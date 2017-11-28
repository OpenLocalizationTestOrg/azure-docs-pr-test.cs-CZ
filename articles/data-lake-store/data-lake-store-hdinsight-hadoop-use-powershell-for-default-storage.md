---
title: "aaaCreate HDInsight clustery s Data Lake Store jako výchozí úložiště pomocí prostředí PowerShell | Microsoft Docs"
description: "Pomocí prostředí Azure PowerShell toocreate a clusterů HDInsight pomocí Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/08/2017
ms.author: nitinme
ms.openlocfilehash: a5c0ad416da6ad9bd07204af2ebb6b7470916085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a><span data-ttu-id="5e281-103">Vytvoření clusterů HDInsight s Data Lake Store jako výchozí úložiště pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e281-103">Create HDInsight clusters with Data Lake Store as default storage by using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e281-104">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5e281-104">Use hello Azure portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="5e281-105">Pomocí prostředí PowerShell (pro výchozí úložiště)</span><span class="sxs-lookup"><span data-stu-id="5e281-105">Use PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="5e281-106">Pomocí prostředí PowerShell (pro další úložiště)</span><span class="sxs-lookup"><span data-stu-id="5e281-106">Use PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="5e281-107">Pomocí Správce prostředků</span><span class="sxs-lookup"><span data-stu-id="5e281-107">Use Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

<span data-ttu-id="5e281-108">Zjistěte, jak toouse prostředí Azure PowerShell tooconfigure Azure HDInsight clustery s Azure Data Lake Store, jako výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="5e281-108">Learn how toouse Azure PowerShell tooconfigure Azure HDInsight clusters with Azure Data Lake Store, as default storage.</span></span> <span data-ttu-id="5e281-109">Pokyny týkající se vytvoření clusteru HDInsight s Data Lake Store jako další úložiště najdete v tématu [vytvoření clusteru HDInsight s Data Lake Store jako další úložiště](data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5e281-109">For instructions on creating an HDInsight cluster with Data Lake Store as additional storage, see [Create an HDInsight cluster with Data Lake Store as additional storage](data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

<span data-ttu-id="5e281-110">Zde jsou některé důležité informace týkající se používání HDInsight s Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="5e281-110">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="5e281-111">clustery HDInsight toocreate Hello možnost s přístupem k tooData Lake Store jako výchozí úložiště je k dispozici pro HDInsight verze 3.5 a 3.6.</span><span class="sxs-lookup"><span data-stu-id="5e281-111">hello option toocreate HDInsight clusters with access tooData Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="5e281-112">Hello možnost toocreate clusterů HDInsight pomocí přístup tooData Lake Store jako výchozí úložiště je *není k dispozici* u clusterů HDInsight Premium.</span><span class="sxs-lookup"><span data-stu-id="5e281-112">hello option toocreate HDInsight clusters with access tooData Lake Store as default storage is *not available* for HDInsight Premium clusters.</span></span>

<span data-ttu-id="5e281-113">tooconfigure toowork HDInsight s Data Lake Store pomocí prostředí PowerShell, postupujte podle pokynů hello v části Další pět hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-113">tooconfigure HDInsight toowork with Data Lake Store by using PowerShell, follow hello instructions in hello next five sections.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e281-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5e281-114">Prerequisites</span></span>
<span data-ttu-id="5e281-115">Než začnete tento kurz, ujistěte se, že splňujete hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="5e281-115">Before you begin this tutorial, make sure that you meet hello following requirements:</span></span>

* <span data-ttu-id="5e281-116">**Předplatné Azure**: přejděte příliš[získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5e281-116">**An Azure subscription**: Go too[Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="5e281-117">**Prostředí Azure PowerShell 1.0 nebo vyšší**: najdete v části [jak tooinstall a konfigurace prostředí PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5e281-117">**Azure PowerShell 1.0 or greater**: See [How tooinstall and configure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="5e281-118">**Windows Software Development Kit (SDK)**: tooinstall Windows SDK a přejděte příliš[stáhne a nástroje pro Windows 10](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="5e281-118">**Windows Software Development Kit (SDK)**: tooinstall Windows SDK, go too[Downloads and tools for Windows 10](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="5e281-119">Hello SDK je použité toocreate certifikát zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5e281-119">hello SDK is used toocreate a security certificate.</span></span>
* <span data-ttu-id="5e281-120">**Objekt služby Azure Active Directory**: Tento kurz popisuje, jak toocreate objektu služby ve službě Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5e281-120">**Azure Active Directory service principal**: This tutorial describes how toocreate a service principal in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="5e281-121">Ale toocreate hlavní název služby, musíte být správce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e281-121">However, toocreate a service principal, you must be an Azure AD administrator.</span></span> <span data-ttu-id="5e281-122">Pokud jste správce, můžete přeskočit tento požadavek a pokračovat v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-122">If you are an administrator, you can skip this prerequisite and proceed with hello tutorial.</span></span>

    >[!NOTE]
    ><span data-ttu-id="5e281-123">Pouze v případě, že jste správce Azure AD, vytvořte službu objektu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5e281-123">You can create a service principal only if you are an Azure AD administrator.</span></span> <span data-ttu-id="5e281-124">Správce služby Azure AD musí vytvořte službu objektu zabezpečení před vytvořením clusteru HDInsight s Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="5e281-124">Your Azure AD administrator must create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="5e281-125">Hello instanční objekt musí být vytvořeny pomocí certifikátu, jak je popsáno v [vytvořit objekt služby pomocí certifikátu](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="5e281-125">hello service principal must be created with a certificate, as described in [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>
    >

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="5e281-126">Vytvoření účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="5e281-126">Create a Data Lake Store account</span></span>
<span data-ttu-id="5e281-127">toocreate účet Data Lake Store hello následující:</span><span class="sxs-lookup"><span data-stu-id="5e281-127">toocreate a Data Lake Store account, do hello following:</span></span>

1. <span data-ttu-id="5e281-128">Z plochy otevřete okno prostředí PowerShell a potom zadejte níže zobrazené fragmenty kódu hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-128">From your desktop, open a PowerShell window, and then enter hello snippets below.</span></span> <span data-ttu-id="5e281-129">Pokud jste výzvami toosign v přihlášení jako správci předplatného hello nebo vlastníky.</span><span class="sxs-lookup"><span data-stu-id="5e281-129">When you are prompted toosign in, sign in as one of hello subscription administrators or owners.</span></span> 

        # Sign in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > <span data-ttu-id="5e281-130">Pokud registrace poskytovatele prostředků hello Data Lake Store a došlo k chybě, podobně jako příliš`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid`, vaše předplatné nemusí být seznam povolených adres pro Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="5e281-130">If you register hello Data Lake Store resource provider and receive an error similar too`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid`, your subscription might not be whitelisted for Data Lake Store.</span></span> <span data-ttu-id="5e281-131">tooenable vašeho předplatného Azure pro hello Data Lake Store verzi public preview, postupujte podle pokynů hello v [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5e281-131">tooenable your Azure subscription for hello Data Lake Store public preview, follow hello instructions in [Get started with Azure Data Lake Store by using hello Azure portal](data-lake-store-get-started-portal.md).</span></span>
    >

2. <span data-ttu-id="5e281-132">Účet Data Lake Store je přidruženo ke skupině prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5e281-132">A Data Lake Store account is associated with an Azure resource group.</span></span> <span data-ttu-id="5e281-133">Začněte vytvořením skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5e281-133">Start by creating a resource group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="5e281-134">Měli byste vidět výstup takto:</span><span class="sxs-lookup"><span data-stu-id="5e281-134">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="5e281-135">Vytvoření účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="5e281-135">Create a Data Lake Store account.</span></span> <span data-ttu-id="5e281-136">účet Hello název, který zadáte, musí obsahovat jenom malá písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="5e281-136">hello account name you specify must contain only lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="5e281-137">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="5e281-137">You should see an output like hello following:</span></span>

        ...
        ProvisioningState           : Succeeded
        State                       : Active
        CreationTime                : 5/5/2017 10:53:56 PM
        EncryptionState             : Enabled
        ...
        LastModifiedTime            : 5/5/2017 10:53:56 PM
        Endpoint                    : hdiadlstore.azuredatalakestore.net
        DefaultGroup                :
        Id                          : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp/providers/Microsoft.DataLakeStore/accounts/hdiadlstore
        Name                        : hdiadlstore
        Type                        : Microsoft.DataLakeStore/accounts
        Location                    : East US 2
        Tags                        : {}

4. <span data-ttu-id="5e281-138">Použití Data Lake Store jako výchozí úložiště vyžaduje jste toospecify specifických pro cluster souborů kořenovou cestu toowhich hello zkopírované při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="5e281-138">Using Data Lake Store as default storage requires you toospecify a root path toowhich hello cluster-specific files are copied during cluster creation.</span></span> <span data-ttu-id="5e281-139">toocreate kořenovou cestu, která je **/clustery/hdiadlcluster** ve fragmentu kódu hello pomocí hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="5e281-139">toocreate a root path, which is **/clusters/hdiadlcluster** in hello  snippet, use hello following cmdlets:</span></span>

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a><span data-ttu-id="5e281-140">Nastavení ověřování pro založené na rolích přístup tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="5e281-140">Set up authentication for role-based access tooData Lake Store</span></span>
<span data-ttu-id="5e281-141">Každé předplatné služby Azure souvisí s entitou, Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e281-141">Every Azure subscription is associated with an Azure AD entity.</span></span> <span data-ttu-id="5e281-142">Uživatelů a služeb, přístup k prostředkům předplatné pomocí hello portál Azure nebo hello rozhraní API služby Azure Resource Manager, musí nejprve ověřit pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e281-142">Users and services that access subscription resources by using hello Azure portal or hello Azure Resource Manager API must first authenticate with Azure AD.</span></span> <span data-ttu-id="5e281-143">Přístup uděluje přiřazením příslušné role hello na prostředek služby Azure tooAzure odběry a služby.</span><span class="sxs-lookup"><span data-stu-id="5e281-143">Access is granted tooAzure subscriptions and services by assigning them hello appropriate role on an Azure resource.</span></span> <span data-ttu-id="5e281-144">Objekt služby pro služby, identifikuje hello služby ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e281-144">For services, a service principal identifies hello service in Azure AD.</span></span>

<span data-ttu-id="5e281-145">Tato část ukazuje, jak toogrant aplikace služby, jako je například HDInsight, tooan přístup prostředků Azure (hello účtu Data Lake Store, který jste vytvořili dříve).</span><span class="sxs-lookup"><span data-stu-id="5e281-145">This section illustrates how toogrant an application service, such as HDInsight, access tooan Azure resource (hello Data Lake Store account that you created earlier).</span></span> <span data-ttu-id="5e281-146">To uděláte tak, že vytvoření služby hlavní aplikace hello a přiřazování rolí tooit pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5e281-146">You do so by creating a service principal for hello application and assigning roles tooit via PowerShell.</span></span>

<span data-ttu-id="5e281-147">tooset až ověřování služby Active Directory pro Azure Data Lake, provádějí úlohy hello v hello následující dvě části.</span><span class="sxs-lookup"><span data-stu-id="5e281-147">tooset up Active Directory authentication for Azure Data Lake, perform hello tasks in hello following two sections.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="5e281-148">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="5e281-148">Create a self-signed certificate</span></span>
<span data-ttu-id="5e281-149">Zajistěte, aby byla [Windows SDK](https://dev.windows.com/en-us/downloads) nainstalovala ještě před pokračováním hello kroky v této části.</span><span class="sxs-lookup"><span data-stu-id="5e281-149">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with hello steps in this section.</span></span> <span data-ttu-id="5e281-150">Musíte také vytvořit adresář, jako například *C:\mycertdir*, kde můžete vytvořit certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-150">You must have also created a directory, such as *C:\mycertdir*, where you create hello certificate.</span></span>

1. <span data-ttu-id="5e281-151">Z okna PowerShell text hello, přejděte toohello umístění, kam jste nainstalovali Windows SDK (obvykle *C:\Program Files (x86) \Windows Kits\10\bin\x86*) a použít hello [MakeCert] [ makecert] toocreate nástroj certifikát podepsaný svým držitelem a privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="5e281-151">From hello PowerShell window, go toohello location where you installed Windows SDK (typically, *C:\Program Files (x86)\Windows Kits\10\bin\x86*) and use hello [MakeCert][makecert] utility toocreate a self-signed certificate and a private key.</span></span> <span data-ttu-id="5e281-152">Hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5e281-152">Use hello following commands:</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="5e281-153">Bude výzvami tooenter hello heslo soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="5e281-153">You will be prompted tooenter hello private key password.</span></span> <span data-ttu-id="5e281-154">Po hello příkaz je úspěšně provést, měli byste vidět **CertFile.cer** a **mykey.pvk** v adresáři hello certifikát, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="5e281-154">After hello command is successfully executed, you should see **CertFile.cer** and **mykey.pvk** in hello certificate directory that you specified.</span></span>
2. <span data-ttu-id="5e281-155">Použití hello [Pvk2Pfx] [ pvk2pfx] nástroj tooconvert hello pvk a .cer soubory tento soubor .pfx vytvořený tooa MakeCert.</span><span class="sxs-lookup"><span data-stu-id="5e281-155">Use hello [Pvk2Pfx][pvk2pfx] utility tooconvert hello .pvk and .cer files that MakeCert created tooa .pfx file.</span></span> <span data-ttu-id="5e281-156">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5e281-156">Run hello following command:</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="5e281-157">Po zobrazení výzvy zadejte hello heslo soukromého klíče dříve zadaný.</span><span class="sxs-lookup"><span data-stu-id="5e281-157">When you are prompted, enter hello private key password that you specified earlier.</span></span> <span data-ttu-id="5e281-158">Hodnota zadaná pro hello Hello **-SP** parametr je hello heslo, které jsou přidružené soubor .pfx hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-158">hello value you specify for hello **-po** parameter is hello password that's associated with hello .pfx file.</span></span> <span data-ttu-id="5e281-159">Po příkazu hello byla úspěšně dokončena, měli byste taky vidět **CertFile.pfx** v adresáři hello certifikát, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="5e281-159">After hello command has been completed successfully, you should also see a **CertFile.pfx** in hello certificate directory that you specified.</span></span>

### <a name="create-an-azure-ad-and-a-service-principal"></a><span data-ttu-id="5e281-160">Vytvoření Azure AD a instanční objekt</span><span class="sxs-lookup"><span data-stu-id="5e281-160">Create an Azure AD and a service principal</span></span>
<span data-ttu-id="5e281-161">V této části vytvořit objekt služby pro aplikaci Azure AD, přiřaďte objekt role toohello služby a ověření jako hello instanční objekt tím, že poskytuje certifikát.</span><span class="sxs-lookup"><span data-stu-id="5e281-161">In this section, you create a service principal for an Azure AD application, assign a role toohello service principal, and authenticate as hello service principal by providing a certificate.</span></span> <span data-ttu-id="5e281-162">toocreate aplikace ve službě Azure AD, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="5e281-162">toocreate an application in Azure AD, run hello following commands:</span></span>

1. <span data-ttu-id="5e281-163">Vložte následující rutiny v okně konzoly prostředí PowerShell hello hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-163">Paste hello following cmdlets in hello PowerShell console window.</span></span> <span data-ttu-id="5e281-164">Ujistěte se, že hello hodnota zadaná pro hello **– DisplayName** vlastnost je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="5e281-164">Make sure that hello value you specify for hello **-DisplayName** property is unique.</span></span> <span data-ttu-id="5e281-165">Hello hodnoty pro **– Domovská stránka** a **- IdentiferUris** jsou zástupné hodnoty a nejsou ověřené.</span><span class="sxs-lookup"><span data-stu-id="5e281-165">hello values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter hello password" # This is hello password you specified for hello .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. <span data-ttu-id="5e281-166">Vytvořit objekt služby s použitím ID hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e281-166">Create a service principal by using hello application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="5e281-167">Udělit hello kořenovém adresáři Data Lake Store toohello služby hlavní přístupu a všechny složky hello v hello kořenovou cestu, která jste zadali dříve.</span><span class="sxs-lookup"><span data-stu-id="5e281-167">Grant hello service principal access toohello Data Lake Store root and all hello folders in hello root path that you specified earlier.</span></span> <span data-ttu-id="5e281-168">Použijte hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="5e281-168">Use hello following cmdlets:</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-hello-default-storage"></a><span data-ttu-id="5e281-169">Vytvoření clusteru služby HDInsight Linux s Data Lake Store jako hello výchozí úložiště</span><span class="sxs-lookup"><span data-stu-id="5e281-169">Create an HDInsight Linux cluster with Data Lake Store as hello default storage</span></span>

<span data-ttu-id="5e281-170">V této části vytvoříte clusteru služby HDInsight Hadoop Linux s Data Lake Store jako hello výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="5e281-170">In this section, you create an HDInsight Hadoop Linux cluster with Data Lake Store as hello default storage.</span></span> <span data-ttu-id="5e281-171">Pro tuto verzi hello HDInsight cluster a Data Lake Store musí být ve hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="5e281-171">For this release, hello HDInsight cluster and Data Lake Store must be in hello same location.</span></span>

1. <span data-ttu-id="5e281-172">Načtení ID hello předplatné klienta a uloží jej později toouse.</span><span class="sxs-lookup"><span data-stu-id="5e281-172">Retrieve hello subscription tenant ID, and store it toouse later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. <span data-ttu-id="5e281-173">Vytvoření clusteru HDInsight se hello pomocí hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="5e281-173">Create hello HDInsight cluster by using hello following cmdlets:</span></span>

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.6" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    <span data-ttu-id="5e281-174">Po úspěšném dokončení hello rutinu byste měli vidět výstup obsahující podrobnosti o clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-174">After hello cmdlet has been successfully completed, you should see an output that lists hello cluster details.</span></span>

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-data-lake-store"></a><span data-ttu-id="5e281-175">Spustit testovací úlohy na toouse clusteru HDInsight hello Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="5e281-175">Run test jobs on hello HDInsight cluster toouse Data Lake Store</span></span>
<span data-ttu-id="5e281-176">Po dokončení konfigurace clusteru služby HDInsight, můžete spustit testovací úlohy na něm tooensure, že Data Lake Store můžete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="5e281-176">After you have configured an HDInsight cluster, you can run test jobs on it tooensure that it can access Data Lake Store.</span></span> <span data-ttu-id="5e281-177">toodo tedy spustit toocreate úlohy Hive ukázkové tabulku, která používá hello ukázková data, která je již k dispozici v Data Lake Store v  *<cluster root>/example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="5e281-177">toodo so, run a sample Hive job toocreate a table that uses hello sample data that's already available in Data Lake Store at *<cluster root>/example/data/sample.log*.</span></span>

<span data-ttu-id="5e281-178">V této části vytvoříte připojení Secure Shell (SSH) do hello cluster HDInsight Linux, který jste vytvořili, a spusťte ukázkový dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="5e281-178">In this section, you make a Secure Shell (SSH) connection into hello HDInsight Linux cluster that you created, and then you run a sample Hive query.</span></span>

* <span data-ttu-id="5e281-179">Pokud používáte toomake klienta Windows připojení SSH do clusteru hello, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="5e281-179">If you are using a Windows client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="5e281-180">Pokud používáte klienta toomake Linux připojení SSH do clusteru hello, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="5e281-180">If you are using a Linux client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="5e281-181">Po provedení hello připojení, spusťte hello Hive rozhraní příkazového řádku (CLI) pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5e281-181">After you have made hello connection, start hello Hive command-line interface (CLI) by using hello following command:</span></span>

        hive
2. <span data-ttu-id="5e281-182">Použití hello rozhraní příkazového řádku tooenter hello následující příkazy toocreate novou tabulku s názvem **vozidel** pomocí hello ukázkových dat v Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="5e281-182">Use hello CLI tooenter hello following statements toocreate a new table named **vehicles** by using hello sample data in Data Lake Store:</span></span>

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="5e281-183">V konzole hello SSH, byste měli vidět výstup hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e281-183">You should see hello query output on hello SSH console.</span></span>

    >[!NOTE]
    ><span data-ttu-id="5e281-184">Hello cesta toohello ukázková data v předchozím příkazu CREATE TABLE hello jsou `adl:///example/data/`, kde `adl:///` je kořenový cluster hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-184">hello path toohello sample data in hello preceding CREATE TABLE command is `adl:///example/data/`, where `adl:///` is hello cluster root.</span></span> <span data-ttu-id="5e281-185">Následující příklad hello kořenové hello clusteru, který je uveden v tomto kurzu, příkaz hello je `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span><span class="sxs-lookup"><span data-stu-id="5e281-185">Following hello example of hello cluster root that's specified in this tutorial, hello command is `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span></span> <span data-ttu-id="5e281-186">Můžete buď použít kratší alternativní hello nebo zadejte toohello hello úplnou cestu ke kořenovému adresáři clusteru.</span><span class="sxs-lookup"><span data-stu-id="5e281-186">You can either use hello shorter alternative or provide hello complete path toohello cluster root.</span></span>
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a><span data-ttu-id="5e281-187">Přístup k Data Lake Store pomocí příkazů HDFS</span><span class="sxs-lookup"><span data-stu-id="5e281-187">Access Data Lake Store by using HDFS commands</span></span>
<span data-ttu-id="5e281-188">Po nakonfigurování hello HDInsight clusteru toouse Data Lake Store, můžete použít Hadoop Distributed File System (HDFS) prostředí příkazy tooaccess hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="5e281-188">After you have configured hello HDInsight cluster toouse Data Lake Store, you can use Hadoop Distributed File System (HDFS) shell commands tooaccess hello store.</span></span>

<span data-ttu-id="5e281-189">V této části provedete připojení SSH do hello cluster HDInsight Linux, který jste vytvořili, a spusťte příkazů HDFS hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-189">In this section, you make an SSH connection into hello HDInsight Linux cluster that you created, and then you run hello HDFS commands.</span></span>

* <span data-ttu-id="5e281-190">Pokud používáte toomake klienta Windows připojení SSH do clusteru hello, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="5e281-190">If you are using a Windows client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="5e281-191">Pokud používáte klienta toomake Linux připojení SSH do clusteru hello, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="5e281-191">If you are using a Linux client toomake an SSH connection into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="5e281-192">Po provedení hello připojení, seznam souborů hello v Data Lake Store pomocí následujících příkazů systému souborů HDFS hello.</span><span class="sxs-lookup"><span data-stu-id="5e281-192">After you've made hello connection, list hello files in Data Lake Store by using hello following HDFS file system command.</span></span>

    hdfs dfs -ls adl:///

<span data-ttu-id="5e281-193">Můžete taky hello `hdfs dfs -put` příkaz tooupload některé soubory tooData Lake Store a pak použijte `hdfs dfs -ls` tooverify jestli hello soubory úspěšně se nahrál.</span><span class="sxs-lookup"><span data-stu-id="5e281-193">You can also use hello `hdfs dfs -put` command tooupload some files tooData Lake Store, and then use `hdfs dfs -ls` tooverify whether hello files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="5e281-194">Viz také</span><span class="sxs-lookup"><span data-stu-id="5e281-194">See also</span></span>
* [<span data-ttu-id="5e281-195">Portál Azure: vytvoření toouse clusteru HDInsight Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="5e281-195">Azure portal: Create an HDInsight cluster toouse Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
