---
title: "Vytvoření clusterů HDInsight s Data Lake Store jako výchozí úložiště pomocí prostředí PowerShell | Microsoft Docs"
description: "Pomocí prostředí Azure PowerShell k vytváření a používání clustery HDInsight s Azure Data Lake Store"
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
ms.openlocfilehash: 77eb83b80312eca401e6f60d57ed6a5668ea442e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a><span data-ttu-id="fabd3-103">Vytvoření clusterů HDInsight s Data Lake Store jako výchozí úložiště pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fabd3-103">Create HDInsight clusters with Data Lake Store as default storage by using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fabd3-104">Použití portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fabd3-104">Use the Azure portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="fabd3-105">Pomocí prostředí PowerShell (pro výchozí úložiště)</span><span class="sxs-lookup"><span data-stu-id="fabd3-105">Use PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="fabd3-106">Pomocí prostředí PowerShell (pro další úložiště)</span><span class="sxs-lookup"><span data-stu-id="fabd3-106">Use PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="fabd3-107">Pomocí Správce prostředků</span><span class="sxs-lookup"><span data-stu-id="fabd3-107">Use Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

<span data-ttu-id="fabd3-108">Další informace o použití prostředí Azure PowerShell ke konfiguraci Azure HDInsight clustery s Azure Data Lake Store, jako výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="fabd3-108">Learn how to use Azure PowerShell to configure Azure HDInsight clusters with Azure Data Lake Store, as default storage.</span></span> <span data-ttu-id="fabd3-109">Pokyny týkající se vytvoření clusteru HDInsight s Data Lake Store jako další úložiště najdete v tématu [vytvoření clusteru HDInsight s Data Lake Store jako další úložiště](data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="fabd3-109">For instructions on creating an HDInsight cluster with Data Lake Store as additional storage, see [Create an HDInsight cluster with Data Lake Store as additional storage](data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

<span data-ttu-id="fabd3-110">Zde jsou některé důležité informace týkající se používání HDInsight s Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="fabd3-110">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="fabd3-111">Možnost k vytvoření clusterů HDInsight s přístupem k Data Lake Store jako výchozí úložiště je k dispozici pro HDInsight verze 3.5 a 3.6.</span><span class="sxs-lookup"><span data-stu-id="fabd3-111">The option to create HDInsight clusters with access to Data Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="fabd3-112">Možnost vytvořit HDInsight clustery s přístupem do Data Lake Store jako výchozí úložiště je *není k dispozici* u clusterů HDInsight Premium.</span><span class="sxs-lookup"><span data-stu-id="fabd3-112">The option to create HDInsight clusters with access to Data Lake Store as default storage is *not available* for HDInsight Premium clusters.</span></span>

<span data-ttu-id="fabd3-113">Ke konfiguraci HDInsight pro práci s Data Lake Store pomocí prostředí PowerShell, postupujte podle pokynů v následujících pět částech.</span><span class="sxs-lookup"><span data-stu-id="fabd3-113">To configure HDInsight to work with Data Lake Store by using PowerShell, follow the instructions in the next five sections.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fabd3-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fabd3-114">Prerequisites</span></span>
<span data-ttu-id="fabd3-115">Než začnete tento kurz, ujistěte se, že splňujete následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="fabd3-115">Before you begin this tutorial, make sure that you meet the following requirements:</span></span>

* <span data-ttu-id="fabd3-116">**Předplatné Azure**: přejděte na [získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fabd3-116">**An Azure subscription**: Go to [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="fabd3-117">**Prostředí Azure PowerShell 1.0 nebo vyšší**: najdete v části [postup instalace a konfigurace prostředí PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fabd3-117">**Azure PowerShell 1.0 or greater**: See [How to install and configure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="fabd3-118">**Windows Software Development Kit (SDK)**: Chcete-li nainstalovat sadu Windows SDK, přejděte na [stáhne a nástroje pro Windows 10](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="fabd3-118">**Windows Software Development Kit (SDK)**: To install Windows SDK, go to [Downloads and tools for Windows 10](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="fabd3-119">Sada SDK se používá k vytvoření certifikát zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fabd3-119">The SDK is used to create a security certificate.</span></span>
* <span data-ttu-id="fabd3-120">**Objekt služby Azure Active Directory**: Tento kurz popisuje, jak vytvořit objekt služby v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fabd3-120">**Azure Active Directory service principal**: This tutorial describes how to create a service principal in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="fabd3-121">Pokud chcete vytvořit objekt služby, ale musí být správce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fabd3-121">However, to create a service principal, you must be an Azure AD administrator.</span></span> <span data-ttu-id="fabd3-122">Pokud jste správce, můžete přeskočit tento požadavek a pokračujte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fabd3-122">If you are an administrator, you can skip this prerequisite and proceed with the tutorial.</span></span>

    >[!NOTE]
    ><span data-ttu-id="fabd3-123">Pouze v případě, že jste správce Azure AD, vytvořte službu objektu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fabd3-123">You can create a service principal only if you are an Azure AD administrator.</span></span> <span data-ttu-id="fabd3-124">Správce služby Azure AD musí vytvořte službu objektu zabezpečení před vytvořením clusteru HDInsight s Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fabd3-124">Your Azure AD administrator must create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="fabd3-125">Objekt služby musí být vytvořeny pomocí certifikátu, jak je popsáno v [vytvořit objekt služby pomocí certifikátu](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="fabd3-125">The service principal must be created with a certificate, as described in [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>
    >

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="fabd3-126">Vytvoření účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fabd3-126">Create a Data Lake Store account</span></span>
<span data-ttu-id="fabd3-127">Pokud chcete vytvořit účet Data Lake Store, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="fabd3-127">To create a Data Lake Store account, do the following:</span></span>

1. <span data-ttu-id="fabd3-128">Z plochy otevřete okno prostředí PowerShell a potom zadejte níže zobrazené fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="fabd3-128">From your desktop, open a PowerShell window, and then enter the snippets below.</span></span> <span data-ttu-id="fabd3-129">Když se zobrazí výzva k přihlášení, přihlaste se jako správci předplatného nebo vlastníky.</span><span class="sxs-lookup"><span data-stu-id="fabd3-129">When you are prompted to sign in, sign in as one of the subscription administrators or owners.</span></span> 

        # Sign in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > <span data-ttu-id="fabd3-130">Pokud registrace zprostředkovatele prostředků Data Lake Store a zobrazí se chyba podobná `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, vaše předplatné nemusí být seznam povolených adres pro Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fabd3-130">If you register the Data Lake Store resource provider and receive an error similar to `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, your subscription might not be whitelisted for Data Lake Store.</span></span> <span data-ttu-id="fabd3-131">Aktivujte předplatné Azure pro verzi public preview Data Lake Store, postupujte podle pokynů v [Začínáme s Azure Data Lake Store pomocí portálu Azure](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fabd3-131">To enable your Azure subscription for the Data Lake Store public preview, follow the instructions in [Get started with Azure Data Lake Store by using the Azure portal](data-lake-store-get-started-portal.md).</span></span>
    >

2. <span data-ttu-id="fabd3-132">Účet Data Lake Store je přidruženo ke skupině prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="fabd3-132">A Data Lake Store account is associated with an Azure resource group.</span></span> <span data-ttu-id="fabd3-133">Začněte vytvořením skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="fabd3-133">Start by creating a resource group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="fabd3-134">Měli byste vidět výstup takto:</span><span class="sxs-lookup"><span data-stu-id="fabd3-134">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="fabd3-135">Vytvoření účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="fabd3-135">Create a Data Lake Store account.</span></span> <span data-ttu-id="fabd3-136">Název účtu, který zadáte, musí obsahovat jenom malá písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="fabd3-136">The account name you specify must contain only lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="fabd3-137">Zobrazený výstup by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="fabd3-137">You should see an output like the following:</span></span>

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

4. <span data-ttu-id="fabd3-138">Pomocí Data Lake Store jako výchozí úložiště, musíte určit kořenovou cestu, do které soubory specifických pro cluster se zkopírují při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="fabd3-138">Using Data Lake Store as default storage requires you to specify a root path to which the cluster-specific files are copied during cluster creation.</span></span> <span data-ttu-id="fabd3-139">Chcete-li vytvořit kořenovou cestu, která je **/clustery/hdiadlcluster** v tomto fragmentu kódu pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="fabd3-139">To create a root path, which is **/clusters/hdiadlcluster** in the  snippet, use the following cmdlets:</span></span>

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a><span data-ttu-id="fabd3-140">Nastavení ověřování pro přístup na základě rolí k Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fabd3-140">Set up authentication for role-based access to Data Lake Store</span></span>
<span data-ttu-id="fabd3-141">Každé předplatné služby Azure souvisí s entitou, Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fabd3-141">Every Azure subscription is associated with an Azure AD entity.</span></span> <span data-ttu-id="fabd3-142">Uživatelů a služeb, která přistupují k prostředkům předplatné pomocí portálu Azure nebo rozhraní API služby Azure Resource Manager, musí nejprve ověřit pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fabd3-142">Users and services that access subscription resources by using the Azure portal or the Azure Resource Manager API must first authenticate with Azure AD.</span></span> <span data-ttu-id="fabd3-143">Udělením přístupu k předplatných Azure a služby přiřazením příslušné role na prostředek služby Azure.</span><span class="sxs-lookup"><span data-stu-id="fabd3-143">Access is granted to Azure subscriptions and services by assigning them the appropriate role on an Azure resource.</span></span> <span data-ttu-id="fabd3-144">Objekt služby pro služby, identifikuje služby ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fabd3-144">For services, a service principal identifies the service in Azure AD.</span></span>

<span data-ttu-id="fabd3-145">Tato část ukazuje postup udělení služby aplikace, jako je například HDInsight, přístup k prostředku Azure (účet Data Lake Store, který jste vytvořili dříve).</span><span class="sxs-lookup"><span data-stu-id="fabd3-145">This section illustrates how to grant an application service, such as HDInsight, access to an Azure resource (the Data Lake Store account that you created earlier).</span></span> <span data-ttu-id="fabd3-146">To uděláte tak, že vytvoříte hlavní aplikace a přiřazení rolí k němu pomocí prostředí PowerShell služby.</span><span class="sxs-lookup"><span data-stu-id="fabd3-146">You do so by creating a service principal for the application and assigning roles to it via PowerShell.</span></span>

<span data-ttu-id="fabd3-147">Nastavení ověřování služby Active Directory pro Azure Data Lake, proveďte kroky v následujících dvou částech.</span><span class="sxs-lookup"><span data-stu-id="fabd3-147">To set up Active Directory authentication for Azure Data Lake, perform the tasks in the following two sections.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="fabd3-148">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="fabd3-148">Create a self-signed certificate</span></span>
<span data-ttu-id="fabd3-149">Zajistěte, aby byla [Windows SDK](https://dev.windows.com/en-us/downloads) nainstalovat před provedením kroků v této části.</span><span class="sxs-lookup"><span data-stu-id="fabd3-149">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with the steps in this section.</span></span> <span data-ttu-id="fabd3-150">Musíte také vytvořit adresář, jako například *C:\mycertdir*, kde můžete vytvořit certifikát.</span><span class="sxs-lookup"><span data-stu-id="fabd3-150">You must have also created a directory, such as *C:\mycertdir*, where you create the certificate.</span></span>

1. <span data-ttu-id="fabd3-151">V okně PowerShell přejděte do umístění, kam jste nainstalovali Windows SDK (obvykle *C:\Program Files (x86) \Windows Kits\10\bin\x86*) a použít [MakeCert] [ makecert] nástroj vytvořit certifikát podepsaný svým držitelem a privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="fabd3-151">From the PowerShell window, go to the location where you installed Windows SDK (typically, *C:\Program Files (x86)\Windows Kits\10\bin\x86*) and use the [MakeCert][makecert] utility to create a self-signed certificate and a private key.</span></span> <span data-ttu-id="fabd3-152">Použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="fabd3-152">Use the following commands:</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="fabd3-153">Zobrazí se výzva k zadání heslo soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="fabd3-153">You will be prompted to enter the private key password.</span></span> <span data-ttu-id="fabd3-154">Po provedení příkazu je úspěšně, měli byste vidět **CertFile.cer** a **mykey.pvk** v adresáři certifikát, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="fabd3-154">After the command is successfully executed, you should see **CertFile.cer** and **mykey.pvk** in the certificate directory that you specified.</span></span>
2. <span data-ttu-id="fabd3-155">Použití [Pvk2Pfx] [ pvk2pfx] nástroj pro převod pvk a .cer soubory, které vytvořili MakeCert do souboru .pfx.</span><span class="sxs-lookup"><span data-stu-id="fabd3-155">Use the [Pvk2Pfx][pvk2pfx] utility to convert the .pvk and .cer files that MakeCert created to a .pfx file.</span></span> <span data-ttu-id="fabd3-156">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fabd3-156">Run the following command:</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="fabd3-157">Po zobrazení výzvy zadejte heslo soukromého klíče, který jste dřív zadali.</span><span class="sxs-lookup"><span data-stu-id="fabd3-157">When you are prompted, enter the private key password that you specified earlier.</span></span> <span data-ttu-id="fabd3-158">Hodnota zadaná **-SP** parametr je heslo, které je přidružené k souboru .pfx.</span><span class="sxs-lookup"><span data-stu-id="fabd3-158">The value you specify for the **-po** parameter is the password that's associated with the .pfx file.</span></span> <span data-ttu-id="fabd3-159">Po příkazu byla úspěšně dokončena, měli byste taky vidět **CertFile.pfx** v adresáři certifikát, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="fabd3-159">After the command has been completed successfully, you should also see a **CertFile.pfx** in the certificate directory that you specified.</span></span>

### <a name="create-an-azure-ad-and-a-service-principal"></a><span data-ttu-id="fabd3-160">Vytvoření Azure AD a instanční objekt</span><span class="sxs-lookup"><span data-stu-id="fabd3-160">Create an Azure AD and a service principal</span></span>
<span data-ttu-id="fabd3-161">V této části vytvořit objekt služby pro aplikaci Azure AD, přiřazení role objekt služby a ověření jako objekt služby tím, že poskytuje certifikát.</span><span class="sxs-lookup"><span data-stu-id="fabd3-161">In this section, you create a service principal for an Azure AD application, assign a role to the service principal, and authenticate as the service principal by providing a certificate.</span></span> <span data-ttu-id="fabd3-162">Pokud chcete vytvořit aplikaci ve službě Azure AD, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="fabd3-162">To create an application in Azure AD, run the following commands:</span></span>

1. <span data-ttu-id="fabd3-163">Vložte následující rutiny v okně konzoly prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fabd3-163">Paste the following cmdlets in the PowerShell console window.</span></span> <span data-ttu-id="fabd3-164">Ujistěte se, že hodnota zadáváte pro **– DisplayName** vlastnost je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="fabd3-164">Make sure that the value you specify for the **-DisplayName** property is unique.</span></span> <span data-ttu-id="fabd3-165">Hodnoty pro **– Domovská stránka** a **- IdentiferUris** jsou zástupné hodnoty a nejsou ověřené.</span><span class="sxs-lookup"><span data-stu-id="fabd3-165">The values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

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
2. <span data-ttu-id="fabd3-166">Vytvořit objekt služby pomocí ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="fabd3-166">Create a service principal by using the application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="fabd3-167">Udělte přístup k hlavní službě Data Lake Store kořenové a všechny složky v kořenové cestě, která jste zadali dříve.</span><span class="sxs-lookup"><span data-stu-id="fabd3-167">Grant the service principal access to the Data Lake Store root and all the folders in the root path that you specified earlier.</span></span> <span data-ttu-id="fabd3-168">Pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="fabd3-168">Use the following cmdlets:</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-the-default-storage"></a><span data-ttu-id="fabd3-169">Vytvoření clusteru služby HDInsight Linux s Data Lake Store jako výchozí úložiště</span><span class="sxs-lookup"><span data-stu-id="fabd3-169">Create an HDInsight Linux cluster with Data Lake Store as the default storage</span></span>

<span data-ttu-id="fabd3-170">V této části vytvoříte clusteru služby HDInsight Hadoop Linux s Data Lake Store jako výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="fabd3-170">In this section, you create an HDInsight Hadoop Linux cluster with Data Lake Store as the default storage.</span></span> <span data-ttu-id="fabd3-171">Pro tuto verzi clusteru HDInsight a Data Lake Store musí být ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="fabd3-171">For this release, the HDInsight cluster and Data Lake Store must be in the same location.</span></span>

1. <span data-ttu-id="fabd3-172">Načtení ID předplatného klienta a uloží jej pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="fabd3-172">Retrieve the subscription tenant ID, and store it to use later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. <span data-ttu-id="fabd3-173">Vytvoření clusteru HDInsight pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="fabd3-173">Create the HDInsight cluster by using the following cmdlets:</span></span>

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
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

    <span data-ttu-id="fabd3-174">Po úspěšném dokončení rutinu byste měli vidět výstup, který uvádí podrobnosti o clusteru.</span><span class="sxs-lookup"><span data-stu-id="fabd3-174">After the cmdlet has been successfully completed, you should see an output that lists the cluster details.</span></span>

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-store"></a><span data-ttu-id="fabd3-175">Spuštění testu úloh na clusteru HDInsight pomocí Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fabd3-175">Run test jobs on the HDInsight cluster to use Data Lake Store</span></span>
<span data-ttu-id="fabd3-176">Po dokončení konfigurace clusteru služby HDInsight, můžete spustit testovací úlohy na něm zajistit, že Data Lake Store můžete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="fabd3-176">After you have configured an HDInsight cluster, you can run test jobs on it to ensure that it can access Data Lake Store.</span></span> <span data-ttu-id="fabd3-177">Uděláte to tak, spusťte ukázkové úlohy Hive a vytvořte tabulku, která používá ukázková data, která je již k dispozici v Data Lake Store v  *<cluster root>/example/data/sample.log*.</span><span class="sxs-lookup"><span data-stu-id="fabd3-177">To do so, run a sample Hive job to create a table that uses the sample data that's already available in Data Lake Store at *<cluster root>/example/data/sample.log*.</span></span>

<span data-ttu-id="fabd3-178">V této části vytvoříte připojení Secure Shell (SSH) do clusteru HDInsight Linux, který jste vytvořili, a spusťte ukázkový dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="fabd3-178">In this section, you make a Secure Shell (SSH) connection into the HDInsight Linux cluster that you created, and then you run a sample Hive query.</span></span>

* <span data-ttu-id="fabd3-179">Pokud používáte klienta se systémem Windows k vytvoření připojení SSH do clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="fabd3-179">If you are using a Windows client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="fabd3-180">Pokud používáte klienta Linux vytvoření připojení SSH do clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fabd3-180">If you are using a Linux client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="fabd3-181">Po provedení připojení, spusťte rozhraní příkazového řádku (CLI) Hive pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="fabd3-181">After you have made the connection, start the Hive command-line interface (CLI) by using the following command:</span></span>

        hive
2. <span data-ttu-id="fabd3-182">Pomocí rozhraní příkazového řádku zadejte následující příkazy, čímž umožňuje vytvořit novou tabulku s názvem **vozidel** pomocí ukázkových dat v Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="fabd3-182">Use the CLI to enter the following statements to create a new table named **vehicles** by using the sample data in Data Lake Store:</span></span>

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    <span data-ttu-id="fabd3-183">Měli byste vidět výstup dotazu v konzole SSH.</span><span class="sxs-lookup"><span data-stu-id="fabd3-183">You should see the query output on the SSH console.</span></span>

    >[!NOTE]
    ><span data-ttu-id="fabd3-184">Cesta k vzorovými daty v předchozím příkazu CREATE TABLE je `adl:///example/data/`, kde `adl:///` je kořenový cluster.</span><span class="sxs-lookup"><span data-stu-id="fabd3-184">The path to the sample data in the preceding CREATE TABLE command is `adl:///example/data/`, where `adl:///` is the cluster root.</span></span> <span data-ttu-id="fabd3-185">Následující příklad kořenu clusteru, který je uveden v tomto kurzu příkaz je `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span><span class="sxs-lookup"><span data-stu-id="fabd3-185">Following the example of the cluster root that's specified in this tutorial, the command is `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`.</span></span> <span data-ttu-id="fabd3-186">Můžete buď použít kratší alternativní nebo zadejte úplnou cestu k kořenovém clusteru.</span><span class="sxs-lookup"><span data-stu-id="fabd3-186">You can either use the shorter alternative or provide the complete path to the cluster root.</span></span>
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a><span data-ttu-id="fabd3-187">Přístup k Data Lake Store pomocí příkazů HDFS</span><span class="sxs-lookup"><span data-stu-id="fabd3-187">Access Data Lake Store by using HDFS commands</span></span>
<span data-ttu-id="fabd3-188">Po dokončení konfigurace clusteru HDInsight pomocí Data Lake Store, můžete přístup k obchodu s příkazy prostředí Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="fabd3-188">After you have configured the HDInsight cluster to use Data Lake Store, you can use Hadoop Distributed File System (HDFS) shell commands to access the store.</span></span>

<span data-ttu-id="fabd3-189">V této části provedete připojení SSH do clusteru HDInsight Linux, který jste vytvořili a pak spusťte příkazy HDFS.</span><span class="sxs-lookup"><span data-stu-id="fabd3-189">In this section, you make an SSH connection into the HDInsight Linux cluster that you created, and then you run the HDFS commands.</span></span>

* <span data-ttu-id="fabd3-190">Pokud používáte klienta se systémem Windows k vytvoření připojení SSH do clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="fabd3-190">If you are using a Windows client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="fabd3-191">Pokud používáte klienta Linux vytvoření připojení SSH do clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fabd3-191">If you are using a Linux client to make an SSH connection into the cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="fabd3-192">Po provedení připojení, seznam souborů v Data Lake Store pomocí následujícího příkazu systému souborů HDFS.</span><span class="sxs-lookup"><span data-stu-id="fabd3-192">After you've made the connection, list the files in Data Lake Store by using the following HDFS file system command.</span></span>

    hdfs dfs -ls adl:///

<span data-ttu-id="fabd3-193">Můžete také `hdfs dfs -put` příkazu do Data Lake Store nahrát některé soubory a pak použijte `hdfs dfs -ls` ověřit, zda soubory byly úspěšně nahrál.</span><span class="sxs-lookup"><span data-stu-id="fabd3-193">You can also use the `hdfs dfs -put` command to upload some files to Data Lake Store, and then use `hdfs dfs -ls` to verify whether the files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="fabd3-194">Viz také</span><span class="sxs-lookup"><span data-stu-id="fabd3-194">See also</span></span>
* [<span data-ttu-id="fabd3-195">Portál Azure: vytvoření clusteru HDInsight používat Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="fabd3-195">Azure portal: Create an HDInsight cluster to use Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
