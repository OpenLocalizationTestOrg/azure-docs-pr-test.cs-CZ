---
title: "aaaRestrict přístup pomocí sdílené přístupové podpisy - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak přistupovat k toouse sdílené přístupové podpisy toorestrict HDInsight, toodata ukládají do objektů BLOB úložiště Azure."
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
ms.openlocfilehash: a34a2f8e52e47a15b09f09bc1fc67fc6159ec75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-shared-access-signatures-toorestrict-access-toodata-in-hdinsight"></a><span data-ttu-id="ccc6b-103">Použití Azure úložiště sdílené přístupové podpisy toorestrict přístup toodata v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ccc6b-103">Use Azure Storage Shared Access Signatures toorestrict access toodata in HDInsight</span></span>

<span data-ttu-id="ccc6b-104">HDInsight má úplný přístup toodata v účtech úložiště Azure hello přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-104">HDInsight has full access toodata in hello Azure Storage accounts associated with hello cluster.</span></span> <span data-ttu-id="ccc6b-105">Můžete vytvořit sdílené přístupové podpisy hello blob kontejneru toorestrict přístup toohello data.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-105">You can use Shared Access Signatures on hello blob container toorestrict access toohello data.</span></span> <span data-ttu-id="ccc6b-106">Například tooprovide oprávnění jen pro čtení toohello data.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-106">For example, tooprovide read-only access toohello data.</span></span> <span data-ttu-id="ccc6b-107">Podpisy sdíleného přístupu (SAS) jsou funkce účtů úložiště Azure, která vám umožní toodata toolimit přístup.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-107">Shared Access Signatures (SAS) are a feature of Azure storage accounts that allows you toolimit access toodata.</span></span> <span data-ttu-id="ccc6b-108">Například poskytuje toodata oprávnění jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-108">For example, providing read-only access toodata.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ccc6b-109">Řešení pomocí Apache škálu zvažte použití HDInsight připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-109">For a solution using Apache Ranger, consider using domain-joined HDInsight.</span></span> <span data-ttu-id="ccc6b-110">Další informace najdete v tématu hello [konfigurace připojený k doméně HDInsight](hdinsight-domain-joined-configure.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-110">For more information, see hello [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) document.</span></span>

> [!WARNING]
> <span data-ttu-id="ccc6b-111">HDInsight musí mít plný přístup toohello výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-111">HDInsight must have full access toohello default storage for hello cluster.</span></span>

## <a name="requirements"></a><span data-ttu-id="ccc6b-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ccc6b-112">Requirements</span></span>

* <span data-ttu-id="ccc6b-113">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="ccc6b-113">An Azure subscription</span></span>
* <span data-ttu-id="ccc6b-114">C# nebo Python.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-114">C# or Python.</span></span> <span data-ttu-id="ccc6b-115">Příklad kódu C# je k dispozici jako řešení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-115">C# example code is provided as a Visual Studio solution.</span></span>

  * <span data-ttu-id="ccc6b-116">Visual Studio musí být ve verzi 2013, 2015 nebo 2017</span><span class="sxs-lookup"><span data-stu-id="ccc6b-116">Visual Studio must be version 2013, 2015, or 2017</span></span>
  * <span data-ttu-id="ccc6b-117">Python musí být verze 2.7 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="ccc6b-117">Python must be version 2.7 or higher</span></span>

* <span data-ttu-id="ccc6b-118">Cluster HDInsight se systémem Linux nebo [prostředí Azure PowerShell] [ powershell] – Pokud máte existující cluster založený na Linuxu, můžete použít Ambari tooadd cluster toohello podpis sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-118">A Linux-based HDInsight cluster OR [Azure PowerShell][powershell] - If you have an existing Linux-based cluster, you can use Ambari tooadd a Shared Access Signature toohello cluster.</span></span> <span data-ttu-id="ccc6b-119">Pokud ne, můžete použít Azure PowerShell toocreate cluster a přidání sdíleného přístupového podpisu během vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-119">If not, you can use Azure PowerShell toocreate a cluster and add a Shared Access Signature during cluster creation.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ccc6b-120">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-120">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ccc6b-121">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ccc6b-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="ccc6b-122">Hello příklad soubory z [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="ccc6b-122">hello example files from [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span></span> <span data-ttu-id="ccc6b-123">Toto úložiště obsahuje hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-123">This repository contains hello following items:</span></span>

  * <span data-ttu-id="ccc6b-124">Projekt sady Visual Studio, který můžete vytvořit kontejner úložiště, uložené zásady a SAS pro použití s HDInsight</span><span class="sxs-lookup"><span data-stu-id="ccc6b-124">A Visual Studio project that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="ccc6b-125">Skript v jazyce Python, můžete vytvořit kontejner úložiště, uložené zásady a SAS pro použití s HDInsight</span><span class="sxs-lookup"><span data-stu-id="ccc6b-125">A Python script that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="ccc6b-126">Skript prostředí PowerShell, který může vytvářet HDInsight clusteru a nakonfigurujte ji toouse hello SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-126">A PowerShell script that can create a HDInsight cluster and configure it toouse hello SAS.</span></span>

## <a name="shared-access-signatures"></a><span data-ttu-id="ccc6b-127">Sdílené přístupové podpisy</span><span class="sxs-lookup"><span data-stu-id="ccc6b-127">Shared Access Signatures</span></span>

<span data-ttu-id="ccc6b-128">Existují dvě formy sdílené přístupové podpisy:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-128">There are two forms of Shared Access Signatures:</span></span>

* <span data-ttu-id="ccc6b-129">Ad hoc: hello času spuštění, čas vypršení platnosti a oprávnění pro hello SAS nejsou zadány na hello identifikátor URI pro SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-129">Ad hoc: hello start time, expiry time, and permissions for hello SAS are all specified on hello SAS URI.</span></span>

* <span data-ttu-id="ccc6b-130">Uložené zásady přístupu: zásady uložené přístupu je definován na kontejner prostředků, jako je například kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-130">Stored access policy: A stored access policy is defined on a resource container, such as a blob container.</span></span> <span data-ttu-id="ccc6b-131">Zásadu lze použít toomanage omezení pro jeden nebo více sdílených přístupových podpisů.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-131">A policy can be used toomanage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="ccc6b-132">Pokud přidružíte SAS se zásadami přístupu uložené, hello SAS dědí omezení hello – hello času spuštění, čas vypršení platnosti a - definována pro zásady přístupu hello uložené oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-132">When you associate a SAS with a stored access policy, hello SAS inherits hello constraints - hello start time, expiry time, and permissions - defined for hello stored access policy.</span></span>

<span data-ttu-id="ccc6b-133">Hello rozdíl mezi formuláři hello dva je důležité pro jeden klíč scénář: odvolaných certifikátů.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-133">hello difference between hello two forms is important for one key scenario: revocation.</span></span> <span data-ttu-id="ccc6b-134">SAS je adresu URL, takže každý, kdo získává hello SAS je můžete použít, bez ohledu na to, kdo ho požadovaný toobegin s.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-134">A SAS is a URL, so anyone who obtains hello SAS can use it, regardless of who requested it toobegin with.</span></span> <span data-ttu-id="ccc6b-135">Pokud veřejně publikována SAS, můžete použít každý, vítáme.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-135">If a SAS is published publicly, it can be used by anyone in hello world.</span></span> <span data-ttu-id="ccc6b-136">SAS, který je distribuován je platný, dokud jednu ze čtyř akcí se stane:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-136">A SAS that is distributed is valid until one of four things happens:</span></span>

1. <span data-ttu-id="ccc6b-137">čas vypršení platnosti Hello zadaný na hello dosaženo SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-137">hello expiry time specified on hello SAS is reached.</span></span>

2. <span data-ttu-id="ccc6b-138">čas vypršení platnosti Hello zadaný v zásadách přístupu hello uložené odkazuje hello dosaženo SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-138">hello expiry time specified on hello stored access policy referenced by hello SAS is reached.</span></span> <span data-ttu-id="ccc6b-139">Hello následující scénáře způsobit čas vypršení platnosti hello toobe dosaženo:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-139">hello following scenarios cause hello expiry time toobe reached:</span></span>

    * <span data-ttu-id="ccc6b-140">časový interval Hello uplynul.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-140">hello time interval has elapsed.</span></span>
    * <span data-ttu-id="ccc6b-141">zásady přístupu Hello uložené je upravený toohave čas vypršení platnosti v posledních hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-141">hello stored access policy is modified toohave an expiry time in hello past.</span></span> <span data-ttu-id="ccc6b-142">Změna hello čas vypršení platnosti je jedním ze způsobů toorevoke hello SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-142">Changing hello expiry time is one way toorevoke hello SAS.</span></span>

3. <span data-ttu-id="ccc6b-143">Hello uložené zásady přístupu odkazuje hello odstranění SAS, který je jiný způsob toorevoke hello SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-143">hello stored access policy referenced by hello SAS is deleted, which is another way toorevoke hello SAS.</span></span> <span data-ttu-id="ccc6b-144">Pokud je znovu vytvořit zásady přístupu hello uložené s hello stejný název, všechny tokeny SAS pro předchozí zásady hello jsou platné (Pokud hello čas vypršení platnosti na hello neuplynul SAS).</span><span class="sxs-lookup"><span data-stu-id="ccc6b-144">If you recreate hello stored access policy with hello same name, all  SAS tokens for hello previous policy are valid (if hello expiry time on hello SAS has not passed).</span></span> <span data-ttu-id="ccc6b-145">Pokud hodláte toorevoke hello SAS, mít jistotu toouse jiný název znovu hello zásady přístupu, doba vypršení platnosti v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-145">If you intend toorevoke hello SAS, be sure toouse a different name if you recreate hello access policy with an expiry time in hello future.</span></span>

4. <span data-ttu-id="ccc6b-146">Hello klíč účtu, který byl použité toocreate hello SAS vygenerován znovu.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-146">hello account key that was used toocreate hello SAS is regenerated.</span></span> <span data-ttu-id="ccc6b-147">Opakované generování klíče hello způsobí, že všechny aplikace, které používají hello předchozí klíče toofail ověřování.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-147">Regenerating hello key causes all applications that use hello previous key toofail authentication.</span></span> <span data-ttu-id="ccc6b-148">Aktualizujte všechny součásti toohello nový klíč.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-148">Update all components toohello new key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ccc6b-149">Sdílený přístupový podpis identifikátor URI je přidružen hello účet klíče používané toocreate hello podpisu a hello související zásady uložené přístupu (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="ccc6b-149">A shared access signature URI is associated with hello account key used toocreate hello signature, and hello associated stored access policy (if any).</span></span> <span data-ttu-id="ccc6b-150">Pokud je zadána žádná zásada uložené přístup, hello pouze způsob toorevoke sdílený přístupový podpis je klíč účtu toochange hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-150">If no stored access policy is specified, hello only way toorevoke a shared access signature is toochange hello account key.</span></span>

<span data-ttu-id="ccc6b-151">Doporučujeme vždy použít zásady uložené přístupu.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-151">We recommend that you always use stored access policies.</span></span> <span data-ttu-id="ccc6b-152">Pokud používáte uložené zásady, můžete odvolat podpisy nebo prodloužit datum vypršení platnosti hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-152">When using stored policies, you can either revoke signatures or extend hello expiry date as needed.</span></span> <span data-ttu-id="ccc6b-153">Hello kroky v tomto dokumentu používají uložené přístup zásady toogenerate SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-153">hello steps in this document use stored access policies toogenerate SAS.</span></span>

<span data-ttu-id="ccc6b-154">Další informace o sdílených přístupových podpisů najdete v tématu [vysvětlení modelu SAS hello](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="ccc6b-154">For more information on Shared Access Signatures, see [Understanding hello SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

### <a name="create-a-stored-policy-and-sas-using-c"></a><span data-ttu-id="ccc6b-155">Vytvoření uložené zásady a SAS pomocí jazyka C\#</span><span class="sxs-lookup"><span data-stu-id="ccc6b-155">Create a stored policy and SAS using C\#</span></span>

1. <span data-ttu-id="ccc6b-156">Otevřete hello řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-156">Open hello solution in Visual Studio.</span></span>

2. <span data-ttu-id="ccc6b-157">V Průzkumníku řešení klikněte pravým tlačítkem na hello **SASToken** projektu a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-157">In Solution Explorer, right-click on hello **SASToken** project and select **Properties**.</span></span>

3. <span data-ttu-id="ccc6b-158">Vyberte **nastavení** a přidejte hodnoty pro hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-158">Select **Settings** and add values for hello following entries:</span></span>

   * <span data-ttu-id="ccc6b-159">StorageConnectionString: hello připojovací řetězec pro hello účet úložiště, které chcete toocreate uložené zásady a SAS pro.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-159">StorageConnectionString: hello connection string for hello storage account that you want toocreate a stored policy and SAS for.</span></span> <span data-ttu-id="ccc6b-160">Hello formát by měl být `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` kde `myaccount` je hello název účtu úložiště a `mykey` je hello klíč pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-160">hello format should be `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` where `myaccount` is hello name of your storage account and `mykey` is hello key for hello storage account.</span></span>

   * <span data-ttu-id="ccc6b-161">ContainerName: hello kontejneru v účtu úložiště hello, který chcete toorestrict přístup k.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-161">ContainerName: hello container in hello storage account that you want toorestrict access to.</span></span>

   * <span data-ttu-id="ccc6b-162">SASPolicyName: hello název toouse pro hello uložené toocreate zásad.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-162">SASPolicyName: hello name toouse for hello stored policy toocreate.</span></span>

   * <span data-ttu-id="ccc6b-163">FileToUpload: hello cesta tooa soubor, který je nahraný toohello kontejner.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-163">FileToUpload: hello path tooa file that is uploaded toohello container.</span></span>

4. <span data-ttu-id="ccc6b-164">Spusťte projekt hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-164">Run hello project.</span></span> <span data-ttu-id="ccc6b-165">Po vygenerování hello SAS, zobrazí se informace podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-165">Information similar toohello following text is displayed once hello SAS has been generated:</span></span>

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="ccc6b-166">Uložte tokenu zásad hello SAS, název účtu úložiště a název kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-166">Save hello SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="ccc6b-167">Tyto hodnoty se používají při přidružení účtu úložiště hello k vašemu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-167">These values are used when associating hello storage account with your HDInsight cluster.</span></span>

### <a name="create-a-stored-policy-and-sas-using-python"></a><span data-ttu-id="ccc6b-168">Vytvoření uložené zásady a SAS používá Python</span><span class="sxs-lookup"><span data-stu-id="ccc6b-168">Create a stored policy and SAS using Python</span></span>

1. <span data-ttu-id="ccc6b-169">Otevřete soubor SASToken.py hello a změnit hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-169">Open hello SASToken.py file and change hello following values:</span></span>

   * <span data-ttu-id="ccc6b-170">zásady\_název: hello název toouse pro hello uložené toocreate zásad.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-170">policy\_name: hello name toouse for hello stored policy toocreate.</span></span>

   * <span data-ttu-id="ccc6b-171">úložiště\_účet\_název: hello název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-171">storage\_account\_name: hello name of your storage account.</span></span>

   * <span data-ttu-id="ccc6b-172">úložiště\_účet\_klíč: hello klíč pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-172">storage\_account\_key: hello key for hello storage account.</span></span>

   * <span data-ttu-id="ccc6b-173">úložiště\_kontejneru\_název: hello kontejneru v účtu úložiště hello, který chcete toorestrict přístup k.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-173">storage\_container\_name: hello container in hello storage account that you want toorestrict access to.</span></span>

   * <span data-ttu-id="ccc6b-174">Příklad\_souboru\_cesta: hello cesta tooa soubor, který je nahraný toohello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-174">example\_file\_path: hello path tooa file that is uploaded toohello container.</span></span>

2. <span data-ttu-id="ccc6b-175">Spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-175">Run hello script.</span></span> <span data-ttu-id="ccc6b-176">Zobrazuje hello SAS token podobné toohello po dokončení skriptu hello následující text:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-176">It displays hello SAS token similar toohello following text when hello script completes:</span></span>

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="ccc6b-177">Uložte tokenu zásad hello SAS, název účtu úložiště a název kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-177">Save hello SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="ccc6b-178">Tyto hodnoty se používají při přidružení účtu úložiště hello k vašemu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-178">These values are used when associating hello storage account with your HDInsight cluster.</span></span>

## <a name="use-hello-sas-with-hdinsight"></a><span data-ttu-id="ccc6b-179">Použijte hello SAS s HDInsight</span><span class="sxs-lookup"><span data-stu-id="ccc6b-179">Use hello SAS with HDInsight</span></span>

<span data-ttu-id="ccc6b-180">Při vytváření clusteru služby HDInsight, musíte zadat účet primárního úložiště a volitelně můžete zadat další účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-180">When creating an HDInsight cluster, you must specify a primary storage account and you can optionally specify additional storage accounts.</span></span> <span data-ttu-id="ccc6b-181">Obě tyto metody přidávání úložiště vyžadují účty úložiště toohello plný přístup a kontejnerů, které se používají.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-181">Both of these methods of adding storage require full access toohello storage accounts and containers that are used.</span></span>

<span data-ttu-id="ccc6b-182">toouse kontejner sdíleného přístupového podpisu toolimit přístup tooa přidat vlastní položku toohello **core-site** konfiguraci pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-182">toouse a Shared Access Signature toolimit access tooa container, add a custom entry toohello **core-site** configuration for hello cluster.</span></span>

* <span data-ttu-id="ccc6b-183">Pro **založené na Windows** nebo **systémem Linux** clusterů HDInsight, můžete přidat položku hello při vytváření clusteru pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-183">For **Windows-based** or **Linux-based** HDInsight clusters, you can add hello entry during cluster creation using PowerShell.</span></span>
* <span data-ttu-id="ccc6b-184">Pro **systémem Linux** clusterů HDInsight, změnit po vytvoření clusteru pomocí nástroje Ambari konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-184">For **Linux-based** HDInsight clusters, change hello configuration after cluster creation using Ambari.</span></span>

### <a name="create-a-cluster-that-uses-hello-sas"></a><span data-ttu-id="ccc6b-185">Vytvoření clusteru, který používá hello SAS</span><span class="sxs-lookup"><span data-stu-id="ccc6b-185">Create a cluster that uses hello SAS</span></span>

<span data-ttu-id="ccc6b-186">Příklad vytvoření clusteru HDInsight této hello používá SAS je součástí hello `CreateCluster` adresář hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-186">An example of creating an HDInsight cluster that uses hello SAS is included in hello `CreateCluster` directory of hello repository.</span></span> <span data-ttu-id="ccc6b-187">toouse, které ho hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-187">toouse it, use hello following steps:</span></span>

1. <span data-ttu-id="ccc6b-188">Otevřete hello `CreateCluster\HDInsightSAS.ps1` soubor v textovém editoru a upravte hello následující hodnoty od začátku hello hello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-188">Open hello `CreateCluster\HDInsightSAS.ps1` file in a text editor and modify hello following values at hello beginning of hello document.</span></span>

    ```powershell
    # Replace 'mycluster' with hello name of hello cluster toobe created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with hello name of hello group toobe created
    $resourceGroupName = 'myresourcegroup'
    # Replace with hello Azure data center you want toohello cluster toolive in
    $location = 'North Europe'
    # Replace with hello name of hello default storage account toobe created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with hello name of hello SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with hello name of hello SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with hello SAS token generated earlier
    $SASToken = 'sastoken'
    # Set hello number of worker nodes in hello cluster
    $clusterSizeInNodes = 3
    ```

    <span data-ttu-id="ccc6b-189">Můžete například změnit `'mycluster'` toohello název clusteru hello chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-189">For example, change `'mycluster'` toohello name of hello cluster you want toocreate.</span></span> <span data-ttu-id="ccc6b-190">hodnoty Hello SAS by měl odpovídat hello hodnoty z předchozích kroků hello při vytváření účtu úložiště a tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-190">hello SAS values should match hello values from hello previous steps when creating a storage account and SAS token.</span></span>

    <span data-ttu-id="ccc6b-191">Po změně hodnoty hello se hello soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-191">Once you have changed hello values, save hello file.</span></span>

2. <span data-ttu-id="ccc6b-192">Otevření nového příkazového řádku Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-192">Open a new Azure PowerShell prompt.</span></span> <span data-ttu-id="ccc6b-193">Pokud nejste obeznámeni s prostředím Azure PowerShell nebo ho nenainstalovali, přečtěte si téma [nainstalovat a nakonfigurovat Azure PowerShell][powershell].</span><span class="sxs-lookup"><span data-stu-id="ccc6b-193">If you are unfamiliar with Azure PowerShell, or have not installed it, see [Install and configure Azure PowerShell][powershell].</span></span>

1. <span data-ttu-id="ccc6b-194">Z řádku hello použijte následující příkaz tooauthenticate tooyour předplatného Azure hello:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-194">From hello prompt, use hello following command tooauthenticate tooyour Azure subscription:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="ccc6b-195">Pokud budete vyzváni, přihlaste se pomocí hello účet pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-195">When prompted, log in with hello account for your Azure subscription.</span></span>

    <span data-ttu-id="ccc6b-196">Pokud váš účet je spojen s několika předplatných Azure, může být nutné toouse `Select-AzureRmSubscription` tooselect hello předplatné chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-196">If your account is associated with multiple Azure subscriptions, you may need toouse `Select-AzureRmSubscription` tooselect hello subscription you wish toouse.</span></span>

4. <span data-ttu-id="ccc6b-197">Z příkazového řádku hello změnit adresáře toohello `CreateCluster` adresář, který obsahuje soubor HDInsightSAS.ps1 hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-197">From hello prompt, change directories toohello `CreateCluster` directory that contains hello HDInsightSAS.ps1 file.</span></span> <span data-ttu-id="ccc6b-198">Pak použijte následující příkaz toorun hello skriptu hello</span><span class="sxs-lookup"><span data-stu-id="ccc6b-198">Then use hello following command toorun hello script</span></span>

    ```powershell
    .\HDInsightSAS.ps1
    ```

    <span data-ttu-id="ccc6b-199">Jako hello skript se spustí zaprotokoluje příkazovém řádku prostředí PowerShell toohello výstup jako vytvoří hello prostředků skupiny a úložiště účtů.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-199">As hello script runs, it logs output toohello PowerShell prompt as it creates hello resource group and storage accounts.</span></span> <span data-ttu-id="ccc6b-200">Jste uživatelem výzvami tooenter hello HTTP pro hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-200">You are prompted tooenter hello HTTP user for hello HDInsight cluster.</span></span> <span data-ttu-id="ccc6b-201">Tento účet je clusteru toohello přístupu použité toosecure HTTP/s.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-201">This account is used toosecure HTTP/s access toohello cluster.</span></span>

    <span data-ttu-id="ccc6b-202">Při vytváření clusteru se systémem Linux, zobrazí se výzva pro název účtu uživatele SSH a heslo.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-202">If you are creating a Linux-based cluster, you are prompted for an SSH user account name and password.</span></span> <span data-ttu-id="ccc6b-203">Tento účet je přihlášení použité tooremotely toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-203">This account is used tooremotely log in toohello cluster.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="ccc6b-204">Po zobrazení výzvy k hello HTTP/s nebo SSH uživatelské jméno a heslo, je nutné zadat heslo, které splňuje hello následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-204">When prompted for hello HTTP/s or SSH user name and password, you must provide a password that meets hello following criteria:</span></span>
   >
   > * <span data-ttu-id="ccc6b-205">Musí být minimálně 10 znaků.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-205">Must be at least 10 characters in length</span></span>
   > * <span data-ttu-id="ccc6b-206">Musí obsahovat nejméně jednu číslici</span><span class="sxs-lookup"><span data-stu-id="ccc6b-206">Must contain at least one digit</span></span>
   > * <span data-ttu-id="ccc6b-207">Musí obsahovat alespoň jeden jiný než alfanumerický znak</span><span class="sxs-lookup"><span data-stu-id="ccc6b-207">Must contain at least one non-alphanumeric character</span></span>
   > * <span data-ttu-id="ccc6b-208">Musí obsahovat aspoň jedno velké nebo malé písmeno.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-208">Must contain at least one upper or lower case letter</span></span>

<span data-ttu-id="ccc6b-209">Jak dlouho trvá chvíli toocomplete tento skript obvykle přibližně 15 minut.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-209">It takes a while for this script toocomplete, usually around 15 minutes.</span></span> <span data-ttu-id="ccc6b-210">Po dokončení skriptu hello bez chyb, byl vytvořen hello cluster.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-210">When hello script completes without any errors, hello cluster has been created.</span></span>

### <a name="use-hello-sas-with-an-existing-cluster"></a><span data-ttu-id="ccc6b-211">Hello SAS pomocí stávajícího clusteru</span><span class="sxs-lookup"><span data-stu-id="ccc6b-211">Use hello SAS with an existing cluster</span></span>

<span data-ttu-id="ccc6b-212">Pokud máte existující cluster založený na Linuxu, můžete přidat hello SAS toohello **core-site** konfigurace pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-212">If you have an existing Linux-based cluster, you can add hello SAS toohello **core-site** configuration by using hello following steps:</span></span>

1. <span data-ttu-id="ccc6b-213">Otevřete hello Ambari webového uživatelského rozhraní pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-213">Open hello Ambari web UI for your cluster.</span></span> <span data-ttu-id="ccc6b-214">Hello adresa pro tuto stránku je https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-214">hello address for this page is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="ccc6b-215">Po zobrazení výzvy ověřování clusteru toohello pomocí hello jméno správce (správce) a heslo použité při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-215">When prompted, authenticate toohello cluster using hello admin name (admin) and password you used when creating hello cluster.</span></span>

2. <span data-ttu-id="ccc6b-216">Hello levé straně hello webovému uživatelskému rozhraní Ambari, vyberte **HDFS** a pak vyberte hello **konfigurací** kartě uprostřed hello stránku hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-216">From hello left side of hello Ambari web UI, select **HDFS** and then select hello **Configs** tab in hello middle of hello page.</span></span>

3. <span data-ttu-id="ccc6b-217">Vyberte hello **Upřesnit** kartě a potom přejděte najde hello **vlastní základní site** části.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-217">Select hello **Advanced** tab, and then scroll until you find hello **Custom core-site** section.</span></span>

4. <span data-ttu-id="ccc6b-218">Rozbalte hello **vlastní základní site** části potom posuňte toohello end a vyberte hello **přidat vlastnost...**  odkaz.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-218">Expand hello **Custom core-site** section, then scroll toohello end and select hello **Add property...** link.</span></span> <span data-ttu-id="ccc6b-219">Použití hello následující hodnoty pro hello **klíč** a **hodnotu** pole:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-219">Use hello following values for hello **Key** and **Value** fields:</span></span>

   * <span data-ttu-id="ccc6b-220">**Klíč**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="ccc6b-220">**Key**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span></span>
   * <span data-ttu-id="ccc6b-221">**Hodnota**: hello SAS vrácený hello jazyka C# nebo Python aplikace byla spuštěna dříve</span><span class="sxs-lookup"><span data-stu-id="ccc6b-221">**Value**: hello SAS returned by hello C# or Python application you ran previously</span></span>

     <span data-ttu-id="ccc6b-222">Nahraďte **CONTAINERNAME** názvem hello kontejneru, který jste použili s aplikací hello jazyka C# nebo SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-222">Replace **CONTAINERNAME** with hello container name you used with hello C# or SAS application.</span></span> <span data-ttu-id="ccc6b-223">Nahraďte **STORAGEACCOUNTNAME** s názvem účtu úložiště hello jste použili.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-223">Replace **STORAGEACCOUNTNAME** with hello storage account name you used.</span></span>

5. <span data-ttu-id="ccc6b-224">Klikněte na tlačítko hello **přidat** tlačítko toosave tento klíč a hodnotu a pak klikněte na hello **Uložit** tlačítko změny konfigurace toosave hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-224">Click hello **Add** button toosave this key and value, then click hello **Save** button toosave hello configuration changes.</span></span> <span data-ttu-id="ccc6b-225">Po zobrazení výzvy, přidejte popis změny hello ("Přidat přístup k úložišti SAS" třeba) a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-225">When prompted, add a description of hello change ("adding SAS storage access" for example) and then click **Save**.</span></span>

    <span data-ttu-id="ccc6b-226">Klikněte na tlačítko **OK** když byly dokončeny hello změny.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-226">Click **OK** when hello changes have been completed.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="ccc6b-227">Hello změna se projeví až po restartování několik služeb.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-227">You must restart several services before hello change takes effect.</span></span>

6. <span data-ttu-id="ccc6b-228">V hello webovému uživatelskému rozhraní Ambari, vyberte **HDFS** hello seznamu na levé hello a potom vyberte **restartujte všechny** z hello **služby akce** rozevíracím seznamu na pravé hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-228">In hello Ambari web UI, select **HDFS** from hello list on hello left, and then select **Restart All** from hello **Service Actions** drop down list on hello right.</span></span> <span data-ttu-id="ccc6b-229">Po zobrazení výzvy vyberte **zapnout režim údržby** a pak vyberte __Conform restartujte všechny ".</span><span class="sxs-lookup"><span data-stu-id="ccc6b-229">When prompted, select **Turn on maintenance mode** and then select __Conform Restart All".</span></span>

    <span data-ttu-id="ccc6b-230">Tento postup opakujte pro MapReduce2 a YARN.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-230">Repeat this process for MapReduce2 and YARN.</span></span>

7. <span data-ttu-id="ccc6b-231">Po restartování služby hello, vyberte každé z nich a zakázat režimu údržby od hello **služby akce** rozevírací nabídku.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-231">Once hello services have restarted, select each one and disable maintenance mode from hello **Service Actions** drop down.</span></span>

## <a name="test-restricted-access"></a><span data-ttu-id="ccc6b-232">Testování s omezeným přístupem</span><span class="sxs-lookup"><span data-stu-id="ccc6b-232">Test restricted access</span></span>

<span data-ttu-id="ccc6b-233">tooverify, mají omezený přístup, použijte hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-233">tooverify that you have restricted access, use hello following methods:</span></span>

* <span data-ttu-id="ccc6b-234">Pro **založené na Windows** clusterů HDInsight pomocí vzdálené plochy tooconnect toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-234">For **Windows-based** HDInsight clusters, use Remote Desktop tooconnect toohello cluster.</span></span> <span data-ttu-id="ccc6b-235">Další informace najdete v tématu [připojit pomocí protokolu RDP tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="ccc6b-235">For more information, see [Connect tooHDInsight using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

    <span data-ttu-id="ccc6b-236">Po připojení používat hello **Hadoop příkazového řádku** ikony na ploše tooopen hello příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-236">Once connected, use hello **Hadoop Command-Line** icon on hello desktop tooopen a command prompt.</span></span>

* <span data-ttu-id="ccc6b-237">Pro **systémem Linux** clusterů HDInsight pomocí SSH tooconnect toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-237">For **Linux-based** HDInsight clusters, use SSH tooconnect toohello cluster.</span></span> <span data-ttu-id="ccc6b-238">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ccc6b-238">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="ccc6b-239">Po připojení toohello clusteru, použijte následující kroky tooverify, můžete pouze pro čtení a seznam položek v účtu úložiště SAS hello hello:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-239">Once connected toohello cluster, use hello following steps tooverify that you can only read and list items on hello SAS storage account:</span></span>

1. <span data-ttu-id="ccc6b-240">obsah hello toolist hello kontejneru, použijte následující příkaz z řádku hello hello:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-240">toolist hello contents of hello container, use hello following command from hello prompt:</span></span> 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    <span data-ttu-id="ccc6b-241">Nahraďte **SASCONTAINER** s názvem hello hello kontejneru vytvořené pro hello účet úložiště SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-241">Replace **SASCONTAINER** with hello name of hello container created for hello SAS storage account.</span></span> <span data-ttu-id="ccc6b-242">Nahraďte **SASACCOUNTNAME** hello název účtu úložiště hello používá pro hello SAS.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-242">Replace **SASACCOUNTNAME** with hello name of hello storage account used for hello SAS.</span></span>

    <span data-ttu-id="ccc6b-243">Hello seznamu zahrnuje nahrát hello kontejneru a SAS čas vytvoření souboru hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-243">hello list includes hello file uploaded when hello container and SAS were created.</span></span>

2. <span data-ttu-id="ccc6b-244">Použijte následující příkaz tooverify, abyste si přečetli hello obsah souboru hello hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-244">Use hello following command tooverify that you can read hello contents of hello file.</span></span> <span data-ttu-id="ccc6b-245">Nahraďte hello **SASCONTAINER** a **SASACCOUNTNAME** jako v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-245">Replace hello **SASCONTAINER** and **SASACCOUNTNAME** as in hello previous step.</span></span> <span data-ttu-id="ccc6b-246">Nahraďte **FILENAME** s názvem hello hello souboru se zobrazí v předchozí příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-246">Replace **FILENAME** with hello name of hello file displayed in hello previous command:</span></span>

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    <span data-ttu-id="ccc6b-247">Tento příkaz vypíše hello obsah souboru hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-247">This command lists hello contents of hello file.</span></span>

3. <span data-ttu-id="ccc6b-248">Použijte následující příkaz toodownload hello toohello místní systém souborů hello:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-248">Use hello following command toodownload hello file toohello local file system:</span></span>

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    <span data-ttu-id="ccc6b-249">Tento příkaz stáhne hello souboru tooa místního souboru s názvem **testfile.txt**.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-249">This command downloads hello file tooa local file named **testfile.txt**.</span></span>

4. <span data-ttu-id="ccc6b-250">Použití hello následující příkaz tooupload hello místního souboru tooa nový soubor s názvem **testupload.txt** na hello úložiště SAS:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-250">Use hello following command tooupload hello local file tooa new file named **testupload.txt** on hello SAS storage:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    <span data-ttu-id="ccc6b-251">Zobrazí se zpráva podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-251">You receive a message similar toohello following text:</span></span>

        put: java.io.IOException

    <span data-ttu-id="ccc6b-252">K této chybě dojde, protože umístění úložiště hello je čtení + pouze seznam.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-252">This error occurs because hello storage location is read+list only.</span></span> <span data-ttu-id="ccc6b-253">Použijte následující příkaz tooput hello data na hello výchozí úložiště pro cluster hello, který lze zapisovat hello:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-253">Use hello following command tooput hello data on hello default storage for hello cluster, which is writable:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    <span data-ttu-id="ccc6b-254">Tentokrát hello operace by měl úspěšně dokončit.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-254">This time, hello operation should complete successfully.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ccc6b-255">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ccc6b-255">Troubleshooting</span></span>

### <a name="a-task-was-canceled"></a><span data-ttu-id="ccc6b-256">Úloha byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-256">A task was canceled</span></span>

<span data-ttu-id="ccc6b-257">**Příznaky**: při vytváření clusteru pomocí skriptu prostředí PowerShell text hello, se může zobrazit hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-257">**Symptoms**: When creating a cluster using hello PowerShell script, you may receive hello following error message:</span></span>

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

<span data-ttu-id="ccc6b-258">**Příčina**: k této chybě může dojít, pokud použijete heslo pro uživatele správce nebo HTTP hello hello clusteru nebo (pro clustery se systémem Linux) uživatel SSH hello.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-258">**Cause**: This error can occur if you use a password for hello admin/HTTP user for hello cluster, or (for Linux-based clusters) hello SSH user.</span></span>

<span data-ttu-id="ccc6b-259">**Řešení**: použít heslo, které splňuje hello následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-259">**Resolution**: Use a password that meets hello following criteria:</span></span>

* <span data-ttu-id="ccc6b-260">Musí být minimálně 10 znaků.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-260">Must be at least 10 characters in length</span></span>
* <span data-ttu-id="ccc6b-261">Musí obsahovat nejméně jednu číslici</span><span class="sxs-lookup"><span data-stu-id="ccc6b-261">Must contain at least one digit</span></span>
* <span data-ttu-id="ccc6b-262">Musí obsahovat alespoň jeden jiný než alfanumerický znak</span><span class="sxs-lookup"><span data-stu-id="ccc6b-262">Must contain at least one non-alphanumeric character</span></span>
* <span data-ttu-id="ccc6b-263">Musí obsahovat aspoň jedno velké nebo malé písmeno.</span><span class="sxs-lookup"><span data-stu-id="ccc6b-263">Must contain at least one upper or lower case letter</span></span>

## <a name="next-steps"></a><span data-ttu-id="ccc6b-264">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ccc6b-264">Next steps</span></span>

<span data-ttu-id="ccc6b-265">Teď, když jste se naučili, jak tooyour tooadd omezený přístup úložiště clusteru HDInsight, zjistěte další způsoby toowork s daty v clusteru:</span><span class="sxs-lookup"><span data-stu-id="ccc6b-265">Now that you have learned how tooadd limited-access storage tooyour HDInsight cluster, learn other ways toowork with data on your cluster:</span></span>

* [<span data-ttu-id="ccc6b-266">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="ccc6b-266">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ccc6b-267">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="ccc6b-267">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ccc6b-268">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="ccc6b-268">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
