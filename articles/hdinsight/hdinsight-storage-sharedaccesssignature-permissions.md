---
title: "Omezení přístupu pomocí sdílené přístupové podpisy - Azure HDInsight | Microsoft Docs"
description: "Naučte se používat sdílené přístupové podpisy omezit HDInsight přístup k datům uloženým v objektů BLOB služby Azure storage."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: 2e4b1a307fae06c0639d93b9804c6f0f703d5900
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a><span data-ttu-id="a98e6-103">Použití Azure sdílené přístupové podpisy úložiště omezit přístup k datům v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a98e6-103">Use Azure Storage Shared Access Signatures to restrict access to data in HDInsight</span></span>

<span data-ttu-id="a98e6-104">HDInsight má úplný přístup k datům v účtech úložiště Azure, který je přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-104">HDInsight has full access to data in the Azure Storage accounts associated with the cluster.</span></span> <span data-ttu-id="a98e6-105">Sdílené přístupové podpisy na kontejner objektů blob můžete použít k omezení přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="a98e6-105">You can use Shared Access Signatures on the blob container to restrict access to the data.</span></span> <span data-ttu-id="a98e6-106">Chcete-li například zadat jen pro čtení přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="a98e6-106">For example, to provide read-only access to the data.</span></span> <span data-ttu-id="a98e6-107">Podpisy sdíleného přístupu (SAS) jsou funkce účtů úložiště Azure, která vám umožní omezit přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="a98e6-107">Shared Access Signatures (SAS) are a feature of Azure storage accounts that allows you to limit access to data.</span></span> <span data-ttu-id="a98e6-108">Například poskytuje přístup jen pro čtení k datům.</span><span class="sxs-lookup"><span data-stu-id="a98e6-108">For example, providing read-only access to data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a98e6-109">Řešení pomocí Apache škálu zvažte použití HDInsight připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="a98e6-109">For a solution using Apache Ranger, consider using domain-joined HDInsight.</span></span> <span data-ttu-id="a98e6-110">Další informace najdete v tématu [konfigurace připojený k doméně HDInsight](hdinsight-domain-joined-configure.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a98e6-110">For more information, see the [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) document.</span></span>

> [!WARNING]
> <span data-ttu-id="a98e6-111">HDInsight musí mít plný přístup k výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="a98e6-111">HDInsight must have full access to the default storage for the cluster.</span></span>

## <a name="requirements"></a><span data-ttu-id="a98e6-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a98e6-112">Requirements</span></span>

* <span data-ttu-id="a98e6-113">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="a98e6-113">An Azure subscription</span></span>
* <span data-ttu-id="a98e6-114">C# nebo Python.</span><span class="sxs-lookup"><span data-stu-id="a98e6-114">C# or Python.</span></span> <span data-ttu-id="a98e6-115">Příklad kódu C# je k dispozici jako řešení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a98e6-115">C# example code is provided as a Visual Studio solution.</span></span>

  * <span data-ttu-id="a98e6-116">Visual Studio musí být ve verzi 2013, 2015 nebo 2017</span><span class="sxs-lookup"><span data-stu-id="a98e6-116">Visual Studio must be version 2013, 2015, or 2017</span></span>
  * <span data-ttu-id="a98e6-117">Python musí být verze 2.7 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="a98e6-117">Python must be version 2.7 or higher</span></span>

* <span data-ttu-id="a98e6-118">Cluster HDInsight se systémem Linux nebo [prostředí Azure PowerShell] [ powershell] – Pokud máte existující cluster založený na Linuxu, můžete použít Ambari přidat sdíleného přístupového podpisu pro clusteru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-118">A Linux-based HDInsight cluster OR [Azure PowerShell][powershell] - If you have an existing Linux-based cluster, you can use Ambari to add a Shared Access Signature to the cluster.</span></span> <span data-ttu-id="a98e6-119">Pokud ne, Azure PowerShell můžete použít k vytvoření clusteru a přidejte sdíleného přístupového podpisu při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-119">If not, you can use Azure PowerShell to create a cluster and add a Shared Access Signature during cluster creation.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a98e6-120">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="a98e6-120">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a98e6-121">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a98e6-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="a98e6-122">V příkladu souborů z [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="a98e6-122">The example files from [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span></span> <span data-ttu-id="a98e6-123">Toto úložiště obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="a98e6-123">This repository contains the following items:</span></span>

  * <span data-ttu-id="a98e6-124">Projekt sady Visual Studio, který můžete vytvořit kontejner úložiště, uložené zásady a SAS pro použití s HDInsight</span><span class="sxs-lookup"><span data-stu-id="a98e6-124">A Visual Studio project that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="a98e6-125">Skript v jazyce Python, můžete vytvořit kontejner úložiště, uložené zásady a SAS pro použití s HDInsight</span><span class="sxs-lookup"><span data-stu-id="a98e6-125">A Python script that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="a98e6-126">Skript prostředí PowerShell, který můžete vytvořit HDInsight cluster a nakonfigurujte ho na používání sdíleného přístupového podpisu.</span><span class="sxs-lookup"><span data-stu-id="a98e6-126">A PowerShell script that can create a HDInsight cluster and configure it to use the SAS.</span></span>

## <a name="shared-access-signatures"></a><span data-ttu-id="a98e6-127">Sdílené přístupové podpisy</span><span class="sxs-lookup"><span data-stu-id="a98e6-127">Shared Access Signatures</span></span>

<span data-ttu-id="a98e6-128">Existují dvě formy sdílené přístupové podpisy:</span><span class="sxs-lookup"><span data-stu-id="a98e6-128">There are two forms of Shared Access Signatures:</span></span>

* <span data-ttu-id="a98e6-129">Ad hoc: čas zahájení, čas vypršení platnosti a oprávnění pro SAS se všechny zadaný v identifikátoru URI SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-129">Ad hoc: The start time, expiry time, and permissions for the SAS are all specified on the SAS URI.</span></span>

* <span data-ttu-id="a98e6-130">Uložené zásady přístupu: zásady uložené přístupu je definován na kontejner prostředků, jako je například kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a98e6-130">Stored access policy: A stored access policy is defined on a resource container, such as a blob container.</span></span> <span data-ttu-id="a98e6-131">Zásady můžete použít ke správě omezení pro jeden nebo více sdílených přístupových podpisů.</span><span class="sxs-lookup"><span data-stu-id="a98e6-131">A policy can be used to manage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="a98e6-132">Pokud přidružíte SAS se zásadami přístupu uložené, zdědí SAS omezení – čas spuštění, čas vypršení platnosti a - definována pro zásady uložené přístupu oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a98e6-132">When you associate a SAS with a stored access policy, the SAS inherits the constraints - the start time, expiry time, and permissions - defined for the stored access policy.</span></span>

<span data-ttu-id="a98e6-133">Rozdíl mezi dvěma formuláři je důležité pro jeden klíč scénář: odvolaných certifikátů.</span><span class="sxs-lookup"><span data-stu-id="a98e6-133">The difference between the two forms is important for one key scenario: revocation.</span></span> <span data-ttu-id="a98e6-134">SAS je adresu URL, takže každý, kdo získává SAS, můžete použít bez ohledu na to, kdo je požadována na začátku.</span><span class="sxs-lookup"><span data-stu-id="a98e6-134">A SAS is a URL, so anyone who obtains the SAS can use it, regardless of who requested it to begin with.</span></span> <span data-ttu-id="a98e6-135">Pokud veřejně publikována SAS, můžete použít kdokoli na světě.</span><span class="sxs-lookup"><span data-stu-id="a98e6-135">If a SAS is published publicly, it can be used by anyone in the world.</span></span> <span data-ttu-id="a98e6-136">SAS, který je distribuován je platný, dokud jednu ze čtyř akcí se stane:</span><span class="sxs-lookup"><span data-stu-id="a98e6-136">A SAS that is distributed is valid until one of four things happens:</span></span>

1. <span data-ttu-id="a98e6-137">Je dosaženo času vypršení platnosti, zadaný na SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-137">The expiry time specified on the SAS is reached.</span></span>

2. <span data-ttu-id="a98e6-138">Je dosaženo času vypršení platnosti, zadaný v zásadách přístupu uložené odkazuje SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-138">The expiry time specified on the stored access policy referenced by the SAS is reached.</span></span> <span data-ttu-id="a98e6-139">Následující scénáře způsobit čas vypršení platnosti nelze připojit:</span><span class="sxs-lookup"><span data-stu-id="a98e6-139">The following scenarios cause the expiry time to be reached:</span></span>

    * <span data-ttu-id="a98e6-140">Časový interval uplynul.</span><span class="sxs-lookup"><span data-stu-id="a98e6-140">The time interval has elapsed.</span></span>
    * <span data-ttu-id="a98e6-141">Zásady uložené přístupu se upravují tak, aby měl čas vypršení platnosti v minulosti.</span><span class="sxs-lookup"><span data-stu-id="a98e6-141">The stored access policy is modified to have an expiry time in the past.</span></span> <span data-ttu-id="a98e6-142">Změna čas vypršení platnosti je jeden způsob odvolání SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-142">Changing the expiry time is one way to revoke the SAS.</span></span>

3. <span data-ttu-id="a98e6-143">Zásady přístupu uložené odkazuje SAS je odstranit, což je další způsob odvolání SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-143">The stored access policy referenced by the SAS is deleted, which is another way to revoke the SAS.</span></span> <span data-ttu-id="a98e6-144">Pokud budete chtít znovu vytvořit zásady uložené přístupu se stejným názvem, všechny tokeny SAS pro předchozí zásady jsou platné (Pokud neuplynul čas vypršení platnosti na sdíleného přístupového podpisu).</span><span class="sxs-lookup"><span data-stu-id="a98e6-144">If you recreate the stored access policy with the same name, all  SAS tokens for the previous policy are valid (if the expiry time on the SAS has not passed).</span></span> <span data-ttu-id="a98e6-145">Pokud máte v úmyslu odvolání SAS, je nutné použít jiný název, pokud je znovu vytvořit zásady přístupu, doba vypršení platnosti v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="a98e6-145">If you intend to revoke the SAS, be sure to use a different name if you recreate the access policy with an expiry time in the future.</span></span>

4. <span data-ttu-id="a98e6-146">Znovu vygeneruje klíč účtu, který byl použit k vytvoření přidružení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a98e6-146">The account key that was used to create the SAS is regenerated.</span></span> <span data-ttu-id="a98e6-147">Obnovuje se klíč způsobí, že všechny aplikace, které používají předchozí klíč vč. ověřování.</span><span class="sxs-lookup"><span data-stu-id="a98e6-147">Regenerating the key causes all applications that use the previous key to fail authentication.</span></span> <span data-ttu-id="a98e6-148">Aktualizujte všechny součásti na nový klíč.</span><span class="sxs-lookup"><span data-stu-id="a98e6-148">Update all components to the new key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a98e6-149">Sdílený přístupový podpis identifikátor URI je spojena s účet klíč používaný k vytvoření podpisu a přidruženého uložené zásady přístupu (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="a98e6-149">A shared access signature URI is associated with the account key used to create the signature, and the associated stored access policy (if any).</span></span> <span data-ttu-id="a98e6-150">Pokud je zadána žádná zásada uložené přístup, jediný způsob, jak odvolat sdílený přístupový podpis je změnit klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="a98e6-150">If no stored access policy is specified, the only way to revoke a shared access signature is to change the account key.</span></span>

<span data-ttu-id="a98e6-151">Doporučujeme vždy použít zásady uložené přístupu.</span><span class="sxs-lookup"><span data-stu-id="a98e6-151">We recommend that you always use stored access policies.</span></span> <span data-ttu-id="a98e6-152">Pokud používáte uložené zásady, můžete odvolat podpisy nebo prodloužit datum vypršení platnosti, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="a98e6-152">When using stored policies, you can either revoke signatures or extend the expiry date as needed.</span></span> <span data-ttu-id="a98e6-153">Kroky v tomto dokumentu uložené zásady přístupu k vygenerování SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-153">The steps in this document use stored access policies to generate SAS.</span></span>

<span data-ttu-id="a98e6-154">Další informace o sdílených přístupových podpisů najdete v tématu [vysvětlení modelu SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="a98e6-154">For more information on Shared Access Signatures, see [Understanding the SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

### <a name="create-a-stored-policy-and-sas-using-c"></a><span data-ttu-id="a98e6-155">Vytvoření uložené zásady a SAS pomocí jazyka C\#</span><span class="sxs-lookup"><span data-stu-id="a98e6-155">Create a stored policy and SAS using C\#</span></span>

1. <span data-ttu-id="a98e6-156">Otevřete řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a98e6-156">Open the solution in Visual Studio.</span></span>

2. <span data-ttu-id="a98e6-157">V Průzkumníku řešení klikněte pravým tlačítkem na **SASToken** projektu a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="a98e6-157">In Solution Explorer, right-click on the **SASToken** project and select **Properties**.</span></span>

3. <span data-ttu-id="a98e6-158">Vyberte **nastavení** a přidejte hodnoty pro následující položky:</span><span class="sxs-lookup"><span data-stu-id="a98e6-158">Select **Settings** and add values for the following entries:</span></span>

   * <span data-ttu-id="a98e6-159">StorageConnectionString: Připojovací řetězec pro účet úložiště, který chcete vytvořit uložené zásady a SAS pro.</span><span class="sxs-lookup"><span data-stu-id="a98e6-159">StorageConnectionString: The connection string for the storage account that you want to create a stored policy and SAS for.</span></span> <span data-ttu-id="a98e6-160">Musí být ve formátu `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` kde `myaccount` je název účtu úložiště a `mykey` je klíč pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a98e6-160">The format should be `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` where `myaccount` is the name of your storage account and `mykey` is the key for the storage account.</span></span>

   * <span data-ttu-id="a98e6-161">ContainerName: Kontejneru v účtu úložiště, který chcete omezit přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="a98e6-161">ContainerName: The container in the storage account that you want to restrict access to.</span></span>

   * <span data-ttu-id="a98e6-162">SASPolicyName: Název vytvořit pomocí uložené zásady.</span><span class="sxs-lookup"><span data-stu-id="a98e6-162">SASPolicyName: The name to use for the stored policy to create.</span></span>

   * <span data-ttu-id="a98e6-163">FileToUpload: Cesta k souboru, který je odeslat do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-163">FileToUpload: The path to a file that is uploaded to the container.</span></span>

4. <span data-ttu-id="a98e6-164">Spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="a98e6-164">Run the project.</span></span> <span data-ttu-id="a98e6-165">Po vygenerování SAS, zobrazí se informace podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="a98e6-165">Information similar to the following text is displayed once the SAS has been generated:</span></span>

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="a98e6-166">Uložte zásadu token SAS, název účtu úložiště a název kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-166">Save the SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="a98e6-167">Tyto hodnoty se používají při přidružení účtu úložiště k vašemu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a98e6-167">These values are used when associating the storage account with your HDInsight cluster.</span></span>

### <a name="create-a-stored-policy-and-sas-using-python"></a><span data-ttu-id="a98e6-168">Vytvoření uložené zásady a SAS používá Python</span><span class="sxs-lookup"><span data-stu-id="a98e6-168">Create a stored policy and SAS using Python</span></span>

1. <span data-ttu-id="a98e6-169">Otevřete soubor SASToken.py a změňte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a98e6-169">Open the SASToken.py file and change the following values:</span></span>

   * <span data-ttu-id="a98e6-170">zásady\_název: název, který má použít k vytvoření uložené zásady.</span><span class="sxs-lookup"><span data-stu-id="a98e6-170">policy\_name: The name to use for the stored policy to create.</span></span>

   * <span data-ttu-id="a98e6-171">úložiště\_účet\_název: název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a98e6-171">storage\_account\_name: The name of your storage account.</span></span>

   * <span data-ttu-id="a98e6-172">úložiště\_účet\_klíč: klíč pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a98e6-172">storage\_account\_key: The key for the storage account.</span></span>

   * <span data-ttu-id="a98e6-173">úložiště\_kontejneru\_název: kontejneru v účtu úložiště, který chcete omezit přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="a98e6-173">storage\_container\_name: The container in the storage account that you want to restrict access to.</span></span>

   * <span data-ttu-id="a98e6-174">Příklad\_souboru\_cesta: cesta k souboru, který je odeslat do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-174">example\_file\_path: The path to a file that is uploaded to the container.</span></span>

2. <span data-ttu-id="a98e6-175">Spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="a98e6-175">Run the script.</span></span> <span data-ttu-id="a98e6-176">Po dokončení skriptu zobrazí tokenu SAS, podobně jako následující text:</span><span class="sxs-lookup"><span data-stu-id="a98e6-176">It displays the SAS token similar to the following text when the script completes:</span></span>

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="a98e6-177">Uložte zásadu token SAS, název účtu úložiště a název kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-177">Save the SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="a98e6-178">Tyto hodnoty se používají při přidružení účtu úložiště k vašemu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a98e6-178">These values are used when associating the storage account with your HDInsight cluster.</span></span>

## <a name="use-the-sas-with-hdinsight"></a><span data-ttu-id="a98e6-179">Použití SAS s HDInsight</span><span class="sxs-lookup"><span data-stu-id="a98e6-179">Use the SAS with HDInsight</span></span>

<span data-ttu-id="a98e6-180">Při vytváření clusteru služby HDInsight, musíte zadat účet primárního úložiště a volitelně můžete zadat další účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="a98e6-180">When creating an HDInsight cluster, you must specify a primary storage account and you can optionally specify additional storage accounts.</span></span> <span data-ttu-id="a98e6-181">Obě tyto metody přidávání úložiště vyžadují úplný přístup k účtům úložiště a kontejnerů, které se používají.</span><span class="sxs-lookup"><span data-stu-id="a98e6-181">Both of these methods of adding storage require full access to the storage accounts and containers that are used.</span></span>

<span data-ttu-id="a98e6-182">Použití sdíleného přístupového podpisu a omezit přístup do kontejneru, přidat vlastní položku k **core-site** konfiguraci pro daný cluster.</span><span class="sxs-lookup"><span data-stu-id="a98e6-182">To use a Shared Access Signature to limit access to a container, add a custom entry to the **core-site** configuration for the cluster.</span></span>

* <span data-ttu-id="a98e6-183">Pro **založené na Windows** nebo **systémem Linux** clusterů HDInsight, můžete přidat položku při vytváření clusteru pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a98e6-183">For **Windows-based** or **Linux-based** HDInsight clusters, you can add the entry during cluster creation using PowerShell.</span></span>
* <span data-ttu-id="a98e6-184">Pro **systémem Linux** clusterů HDInsight, změnit konfiguraci po vytvoření clusteru pomocí nástroje Ambari.</span><span class="sxs-lookup"><span data-stu-id="a98e6-184">For **Linux-based** HDInsight clusters, change the configuration after cluster creation using Ambari.</span></span>

### <a name="create-a-cluster-that-uses-the-sas"></a><span data-ttu-id="a98e6-185">Vytvoření clusteru, který používá SAS</span><span class="sxs-lookup"><span data-stu-id="a98e6-185">Create a cluster that uses the SAS</span></span>

<span data-ttu-id="a98e6-186">Příklad vytvoření clusteru služby HDInsight, který používá SAS je součástí `CreateCluster` adresáři úložiště.</span><span class="sxs-lookup"><span data-stu-id="a98e6-186">An example of creating an HDInsight cluster that uses the SAS is included in the `CreateCluster` directory of the repository.</span></span> <span data-ttu-id="a98e6-187">Pokud chcete použít, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a98e6-187">To use it, use the following steps:</span></span>

1. <span data-ttu-id="a98e6-188">Otevřete `CreateCluster\HDInsightSAS.ps1` soubor v textovém editoru a upravte následující hodnoty na začátku dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a98e6-188">Open the `CreateCluster\HDInsightSAS.ps1` file in a text editor and modify the following values at the beginning of the document.</span></span>

    ```powershell
    # Replace 'mycluster' with the name of the cluster to be created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with the name of the group to be created
    $resourceGroupName = 'myresourcegroup'
    # Replace with the Azure data center you want to the cluster to live in
    $location = 'North Europe'
    # Replace with the name of the default storage account to be created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with the name of the SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with the name of the SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with the SAS token generated earlier
    $SASToken = 'sastoken'
    # Set the number of worker nodes in the cluster
    $clusterSizeInNodes = 3
    ```

    <span data-ttu-id="a98e6-189">Můžete například změnit `'mycluster'` na název clusteru, kterou chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="a98e6-189">For example, change `'mycluster'` to the name of the cluster you want to create.</span></span> <span data-ttu-id="a98e6-190">Hodnoty SAS by měl odpovídat hodnoty z předchozí kroky při vytváření účtu úložiště a tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-190">The SAS values should match the values from the previous steps when creating a storage account and SAS token.</span></span>

    <span data-ttu-id="a98e6-191">Po změně hodnoty se uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="a98e6-191">Once you have changed the values, save the file.</span></span>

2. <span data-ttu-id="a98e6-192">Otevření nového příkazového řádku Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a98e6-192">Open a new Azure PowerShell prompt.</span></span> <span data-ttu-id="a98e6-193">Pokud nejste obeznámeni s prostředím Azure PowerShell nebo ho nenainstalovali, přečtěte si téma [nainstalovat a nakonfigurovat Azure PowerShell][powershell].</span><span class="sxs-lookup"><span data-stu-id="a98e6-193">If you are unfamiliar with Azure PowerShell, or have not installed it, see [Install and configure Azure PowerShell][powershell].</span></span>

1. <span data-ttu-id="a98e6-194">Na řádku použijte následující příkaz k ověření vašeho předplatného Azure:</span><span class="sxs-lookup"><span data-stu-id="a98e6-194">From the prompt, use the following command to authenticate to your Azure subscription:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="a98e6-195">Pokud budete vyzváni, přihlaste se pomocí účtu pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="a98e6-195">When prompted, log in with the account for your Azure subscription.</span></span>

    <span data-ttu-id="a98e6-196">Pokud váš účet je spojen s několika předplatných Azure, budete možná muset použít `Select-AzureRmSubscription` vyberte předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="a98e6-196">If your account is associated with multiple Azure subscriptions, you may need to use `Select-AzureRmSubscription` to select the subscription you wish to use.</span></span>

4. <span data-ttu-id="a98e6-197">Na řádku přejděte do adresáře `CreateCluster` adresář, který obsahuje soubor HDInsightSAS.ps1.</span><span class="sxs-lookup"><span data-stu-id="a98e6-197">From the prompt, change directories to the `CreateCluster` directory that contains the HDInsightSAS.ps1 file.</span></span> <span data-ttu-id="a98e6-198">Pak použijte následující příkaz pro spuštění skriptu</span><span class="sxs-lookup"><span data-stu-id="a98e6-198">Then use the following command to run the script</span></span>

    ```powershell
    .\HDInsightSAS.ps1
    ```

    <span data-ttu-id="a98e6-199">Jako skript se spustí, zaprotokoluje výstup do příkazového řádku prostředí PowerShell jako vytvoří prostředek skupiny a úložiště účtů.</span><span class="sxs-lookup"><span data-stu-id="a98e6-199">As the script runs, it logs output to the PowerShell prompt as it creates the resource group and storage accounts.</span></span> <span data-ttu-id="a98e6-200">Zobrazí se výzva k zadání uživatele HTTP pro HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="a98e6-200">You are prompted to enter the HTTP user for the HDInsight cluster.</span></span> <span data-ttu-id="a98e6-201">Tento účet slouží k zabezpečení přístup HTTP/s pro cluster.</span><span class="sxs-lookup"><span data-stu-id="a98e6-201">This account is used to secure HTTP/s access to the cluster.</span></span>

    <span data-ttu-id="a98e6-202">Při vytváření clusteru se systémem Linux, zobrazí se výzva pro název účtu uživatele SSH a heslo.</span><span class="sxs-lookup"><span data-stu-id="a98e6-202">If you are creating a Linux-based cluster, you are prompted for an SSH user account name and password.</span></span> <span data-ttu-id="a98e6-203">Tento účet slouží pro vzdálené přihlášení do clusteru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-203">This account is used to remotely log in to the cluster.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="a98e6-204">Po zobrazení výzvy k protokolu HTTP/s nebo SSH uživatelské jméno a heslo, je nutné zadat heslo, které splňuje následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="a98e6-204">When prompted for the HTTP/s or SSH user name and password, you must provide a password that meets the following criteria:</span></span>
   >
   > * <span data-ttu-id="a98e6-205">Musí být minimálně 10 znaků.</span><span class="sxs-lookup"><span data-stu-id="a98e6-205">Must be at least 10 characters in length</span></span>
   > * <span data-ttu-id="a98e6-206">Musí obsahovat nejméně jednu číslici</span><span class="sxs-lookup"><span data-stu-id="a98e6-206">Must contain at least one digit</span></span>
   > * <span data-ttu-id="a98e6-207">Musí obsahovat alespoň jeden jiný než alfanumerický znak</span><span class="sxs-lookup"><span data-stu-id="a98e6-207">Must contain at least one non-alphanumeric character</span></span>
   > * <span data-ttu-id="a98e6-208">Musí obsahovat aspoň jedno velké nebo malé písmeno.</span><span class="sxs-lookup"><span data-stu-id="a98e6-208">Must contain at least one upper or lower case letter</span></span>

<span data-ttu-id="a98e6-209">Jak dlouho trvá chvíli pro tento skript k dokončení, obvykle přibližně 15 minut.</span><span class="sxs-lookup"><span data-stu-id="a98e6-209">It takes a while for this script to complete, usually around 15 minutes.</span></span> <span data-ttu-id="a98e6-210">Po dokončení skriptu bez chyb, byl vytvořen cluster.</span><span class="sxs-lookup"><span data-stu-id="a98e6-210">When the script completes without any errors, the cluster has been created.</span></span>

### <a name="use-the-sas-with-an-existing-cluster"></a><span data-ttu-id="a98e6-211">Použití SAS s stávajícího clusteru</span><span class="sxs-lookup"><span data-stu-id="a98e6-211">Use the SAS with an existing cluster</span></span>

<span data-ttu-id="a98e6-212">Pokud máte existující cluster založený na Linuxu, můžete přidat SAS k **core-site** konfigurace pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="a98e6-212">If you have an existing Linux-based cluster, you can add the SAS to the **core-site** configuration by using the following steps:</span></span>

1. <span data-ttu-id="a98e6-213">Otevřete webovému uživatelskému rozhraní Ambari pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="a98e6-213">Open the Ambari web UI for your cluster.</span></span> <span data-ttu-id="a98e6-214">Adresa pro tuto stránku je https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="a98e6-214">The address for this page is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="a98e6-215">Po zobrazení výzvy ověřování ke clusteru pomocí jméno správce (správce) a heslo použité při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-215">When prompted, authenticate to the cluster using the admin name (admin) and password you used when creating the cluster.</span></span>

2. <span data-ttu-id="a98e6-216">Na levé straně webovému uživatelskému rozhraní Ambari, vyberte **HDFS** a pak vyberte **konfigurací** kartě uprostřed stránky.</span><span class="sxs-lookup"><span data-stu-id="a98e6-216">From the left side of the Ambari web UI, select **HDFS** and then select the **Configs** tab in the middle of the page.</span></span>

3. <span data-ttu-id="a98e6-217">Vyberte **Upřesnit** kartě a poté vyhledejte **vlastní základní site** části.</span><span class="sxs-lookup"><span data-stu-id="a98e6-217">Select the **Advanced** tab, and then scroll until you find the **Custom core-site** section.</span></span>

4. <span data-ttu-id="a98e6-218">Rozbalte **vlastní základní site** oddíl a potom přejděte na koncové a vyberte **přidat vlastnost...**  odkaz.</span><span class="sxs-lookup"><span data-stu-id="a98e6-218">Expand the **Custom core-site** section, then scroll to the end and select the **Add property...** link.</span></span> <span data-ttu-id="a98e6-219">Použijte následující hodnoty pro **klíč** a **hodnotu** pole:</span><span class="sxs-lookup"><span data-stu-id="a98e6-219">Use the following values for the **Key** and **Value** fields:</span></span>

   * <span data-ttu-id="a98e6-220">**Klíč**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="a98e6-220">**Key**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span></span>
   * <span data-ttu-id="a98e6-221">**Hodnota**: SAS vrácený jazyka C# nebo Python aplikace byla spuštěna dříve</span><span class="sxs-lookup"><span data-stu-id="a98e6-221">**Value**: The SAS returned by the C# or Python application you ran previously</span></span>

     <span data-ttu-id="a98e6-222">Nahraďte **CONTAINERNAME** s název kontejneru, který jste použili k aplikaci C# nebo SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-222">Replace **CONTAINERNAME** with the container name you used with the C# or SAS application.</span></span> <span data-ttu-id="a98e6-223">Nahraďte **STORAGEACCOUNTNAME** názvem účtu úložiště, který jste použili.</span><span class="sxs-lookup"><span data-stu-id="a98e6-223">Replace **STORAGEACCOUNTNAME** with the storage account name you used.</span></span>

5. <span data-ttu-id="a98e6-224">Klikněte **přidat** tlačítko Uložit tento klíč a hodnotu a pak klikněte na **Uložit** tlačítko Uložit změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a98e6-224">Click the **Add** button to save this key and value, then click the **Save** button to save the configuration changes.</span></span> <span data-ttu-id="a98e6-225">Po zobrazení výzvy, přidejte popis změny ("Přidat přístup k úložišti SAS" třeba) a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a98e6-225">When prompted, add a description of the change ("adding SAS storage access" for example) and then click **Save**.</span></span>

    <span data-ttu-id="a98e6-226">Klikněte na tlačítko **OK** když byly dokončeny změny.</span><span class="sxs-lookup"><span data-stu-id="a98e6-226">Click **OK** when the changes have been completed.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="a98e6-227">Změna se projeví až po restartování několik služeb.</span><span class="sxs-lookup"><span data-stu-id="a98e6-227">You must restart several services before the change takes effect.</span></span>

6. <span data-ttu-id="a98e6-228">V Ambari webového uživatelského rozhraní, vyberte **HDFS** ze seznamu na levé straně a potom vyberte **restartujte všechny** z **služby akce** rozevíracím seznamu na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="a98e6-228">In the Ambari web UI, select **HDFS** from the list on the left, and then select **Restart All** from the **Service Actions** drop down list on the right.</span></span> <span data-ttu-id="a98e6-229">Po zobrazení výzvy vyberte **zapnout režim údržby** a pak vyberte __Conform restartujte všechny ".</span><span class="sxs-lookup"><span data-stu-id="a98e6-229">When prompted, select **Turn on maintenance mode** and then select __Conform Restart All".</span></span>

    <span data-ttu-id="a98e6-230">Tento postup opakujte pro MapReduce2 a YARN.</span><span class="sxs-lookup"><span data-stu-id="a98e6-230">Repeat this process for MapReduce2 and YARN.</span></span>

7. <span data-ttu-id="a98e6-231">Po restartování služby, vyberte každé z nich a zakázat režimu údržby od **služby akce** rozevírací nabídku.</span><span class="sxs-lookup"><span data-stu-id="a98e6-231">Once the services have restarted, select each one and disable maintenance mode from the **Service Actions** drop down.</span></span>

## <a name="test-restricted-access"></a><span data-ttu-id="a98e6-232">Testování s omezeným přístupem</span><span class="sxs-lookup"><span data-stu-id="a98e6-232">Test restricted access</span></span>

<span data-ttu-id="a98e6-233">Pokud chcete ověřit, že mají omezený přístup, použijte následující metody:</span><span class="sxs-lookup"><span data-stu-id="a98e6-233">To verify that you have restricted access, use the following methods:</span></span>

* <span data-ttu-id="a98e6-234">Pro **založené na Windows** clustery HDInsight k připojení ke clusteru pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="a98e6-234">For **Windows-based** HDInsight clusters, use Remote Desktop to connect to the cluster.</span></span> <span data-ttu-id="a98e6-235">Další informace najdete v tématu [připojení k HDInsight pomocí protokolu RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="a98e6-235">For more information, see [Connect to HDInsight using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

    <span data-ttu-id="a98e6-236">Po připojení používat **Hadoop příkazového řádku** ikony na ploše otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="a98e6-236">Once connected, use the **Hadoop Command-Line** icon on the desktop to open a command prompt.</span></span>

* <span data-ttu-id="a98e6-237">Pro **systémem Linux** clusterů HDInsight, připojte se ke clusteru pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="a98e6-237">For **Linux-based** HDInsight clusters, use SSH to connect to the cluster.</span></span> <span data-ttu-id="a98e6-238">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="a98e6-238">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="a98e6-239">Po připojení ke clusteru pomocí následujících kroků ověřte, můžete na účtu úložiště SAS pouze pro čtení a seznam položek:</span><span class="sxs-lookup"><span data-stu-id="a98e6-239">Once connected to the cluster, use the following steps to verify that you can only read and list items on the SAS storage account:</span></span>

1. <span data-ttu-id="a98e6-240">K zobrazení seznamu obsahu kontejneru, použijte následující příkaz z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="a98e6-240">To list the contents of the container, use the following command from the prompt:</span></span> 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    <span data-ttu-id="a98e6-241">Nahraďte **SASCONTAINER** s názvem kontejneru vytvořili pro účet úložiště SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-241">Replace **SASCONTAINER** with the name of the container created for the SAS storage account.</span></span> <span data-ttu-id="a98e6-242">Nahraďte **SASACCOUNTNAME** s názvem účet úložiště používané pro SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-242">Replace **SASACCOUNTNAME** with the name of the storage account used for the SAS.</span></span>

    <span data-ttu-id="a98e6-243">Seznam obsahuje soubor nahrát při vytvoření kontejneru a SAS.</span><span class="sxs-lookup"><span data-stu-id="a98e6-243">The list includes the file uploaded when the container and SAS were created.</span></span>

2. <span data-ttu-id="a98e6-244">Použijte následující příkaz k ověření, že si můžete přečíst obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-244">Use the following command to verify that you can read the contents of the file.</span></span> <span data-ttu-id="a98e6-245">Nahraďte **SASCONTAINER** a **SASACCOUNTNAME** jako v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="a98e6-245">Replace the **SASCONTAINER** and **SASACCOUNTNAME** as in the previous step.</span></span> <span data-ttu-id="a98e6-246">Nahraďte **FILENAME** s názvem souboru se zobrazí v předchozí příkaz:</span><span class="sxs-lookup"><span data-stu-id="a98e6-246">Replace **FILENAME** with the name of the file displayed in the previous command:</span></span>

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    <span data-ttu-id="a98e6-247">Tento příkaz vypíše obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="a98e6-247">This command lists the contents of the file.</span></span>

3. <span data-ttu-id="a98e6-248">Stáhněte soubor do místního systému souborů pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="a98e6-248">Use the following command to download the file to the local file system:</span></span>

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    <span data-ttu-id="a98e6-249">Tento příkaz stáhne soubor do místního souboru s názvem **testfile.txt**.</span><span class="sxs-lookup"><span data-stu-id="a98e6-249">This command downloads the file to a local file named **testfile.txt**.</span></span>

4. <span data-ttu-id="a98e6-250">Použít následující příkaz k nahrání do nový soubor s názvem místního souboru **testupload.txt** na úložiště SAS:</span><span class="sxs-lookup"><span data-stu-id="a98e6-250">Use the following command to upload the local file to a new file named **testupload.txt** on the SAS storage:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    <span data-ttu-id="a98e6-251">Zobrazí zpráva podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="a98e6-251">You receive a message similar to the following text:</span></span>

        put: java.io.IOException

    <span data-ttu-id="a98e6-252">K této chybě dojde, protože umístění úložiště je pro čtení + pouze seznam.</span><span class="sxs-lookup"><span data-stu-id="a98e6-252">This error occurs because the storage location is read+list only.</span></span> <span data-ttu-id="a98e6-253">Umístění dat na výchozí úložiště pro cluster, který lze zapisovat, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a98e6-253">Use the following command to put the data on the default storage for the cluster, which is writable:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    <span data-ttu-id="a98e6-254">Tento čas by měl úspěšně dokončit operaci.</span><span class="sxs-lookup"><span data-stu-id="a98e6-254">This time, the operation should complete successfully.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a98e6-255">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a98e6-255">Troubleshooting</span></span>

### <a name="a-task-was-canceled"></a><span data-ttu-id="a98e6-256">Úloha byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="a98e6-256">A task was canceled</span></span>

<span data-ttu-id="a98e6-257">**Příznaky**: při vytváření clusteru pomocí skriptu prostředí PowerShell, zobrazí se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="a98e6-257">**Symptoms**: When creating a cluster using the PowerShell script, you may receive the following error message:</span></span>

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

<span data-ttu-id="a98e6-258">**Příčina**: k této chybě může dojít, pokud použijete heslo pro uživatele správce nebo HTTP pro cluster nebo (pro clustery se systémem Linux) uživatel SSH.</span><span class="sxs-lookup"><span data-stu-id="a98e6-258">**Cause**: This error can occur if you use a password for the admin/HTTP user for the cluster, or (for Linux-based clusters) the SSH user.</span></span>

<span data-ttu-id="a98e6-259">**Řešení**: použít heslo, které splňuje následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="a98e6-259">**Resolution**: Use a password that meets the following criteria:</span></span>

* <span data-ttu-id="a98e6-260">Musí být minimálně 10 znaků.</span><span class="sxs-lookup"><span data-stu-id="a98e6-260">Must be at least 10 characters in length</span></span>
* <span data-ttu-id="a98e6-261">Musí obsahovat nejméně jednu číslici</span><span class="sxs-lookup"><span data-stu-id="a98e6-261">Must contain at least one digit</span></span>
* <span data-ttu-id="a98e6-262">Musí obsahovat alespoň jeden jiný než alfanumerický znak</span><span class="sxs-lookup"><span data-stu-id="a98e6-262">Must contain at least one non-alphanumeric character</span></span>
* <span data-ttu-id="a98e6-263">Musí obsahovat aspoň jedno velké nebo malé písmeno.</span><span class="sxs-lookup"><span data-stu-id="a98e6-263">Must contain at least one upper or lower case letter</span></span>

## <a name="next-steps"></a><span data-ttu-id="a98e6-264">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a98e6-264">Next steps</span></span>

<span data-ttu-id="a98e6-265">Teď, když jste se naučili postup přidání úložiště omezený přístup ke svému clusteru HDInsight, se naučíte další způsoby, jak pracovat s daty v clusteru:</span><span class="sxs-lookup"><span data-stu-id="a98e6-265">Now that you have learned how to add limited-access storage to your HDInsight cluster, learn other ways to work with data on your cluster:</span></span>

* [<span data-ttu-id="a98e6-266">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="a98e6-266">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="a98e6-267">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="a98e6-267">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="a98e6-268">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="a98e6-268">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
