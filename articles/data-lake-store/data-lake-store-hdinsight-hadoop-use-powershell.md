---
<span data-ttu-id="601cc-101">Title: aaa "prostředí PowerShell: cluster Azure HDInsight s Data Lake Store jako přídavná služba storage | Služby Microsoft Docs": data lake store, hdinsight documentationcenter:" Autor: nitinme správce: jhubbard editor: cgronlun</span><span class="sxs-lookup"><span data-stu-id="601cc-101">title: aaa"PowerShell: Azure HDInsight cluster with Data Lake Store as add-on storage | Microsoft Docs" services: data-lake-store,hdinsight documentationcenter: '' author: nitinme manager: jhubbard editor: cgronlun</span></span>

<span data-ttu-id="601cc-102">MS.AssetID: 164ada5a-222e-4be2-bd32-e51dbe993bc0 ms.service: úložiště dat lake ms.devlang: na ms.topic: článek ms.tgt_pltfrm: na ms.workload: velkých objemů dat ms.date: ms.author 06/08/2017: nitinme</span><span class="sxs-lookup"><span data-stu-id="601cc-102">ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0 ms.service: data-lake-store ms.devlang: na ms.topic: article ms.tgt_pltfrm: na ms.workload: big-data ms.date: 06/08/2017 ms.author: nitinme</span></span>

---
# <a name="use-azure-powershell-toocreate-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="601cc-103">Použití Azure PowerShell toocreate clusteru HDInsight s Data Lake Store (jako další úložiště)</span><span class="sxs-lookup"><span data-stu-id="601cc-103">Use Azure PowerShell toocreate an HDInsight cluster with Data Lake Store (as additional storage)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="601cc-104">Pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="601cc-104">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="601cc-105">Pomocí prostředí PowerShell (pro výchozí úložiště)</span><span class="sxs-lookup"><span data-stu-id="601cc-105">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="601cc-106">Pomocí prostředí PowerShell (pro další úložiště)</span><span class="sxs-lookup"><span data-stu-id="601cc-106">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="601cc-107">Pomocí Správce prostředků</span><span class="sxs-lookup"><span data-stu-id="601cc-107">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="601cc-108">Zjistěte, jak tooconfigure toouse prostředí Azure PowerShell HDInsight cluster s Azure Data Lake Store, **jako další úložiště**.</span><span class="sxs-lookup"><span data-stu-id="601cc-108">Learn how toouse Azure PowerShell tooconfigure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span> <span data-ttu-id="601cc-109">Pokyny jak toocreate HDInsight, cluster s Azure Data Lake Store jako výchozí úložiště, najdete v části [vytvoření clusteru HDInsight s Data Lake Store jako výchozí úložiště](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span><span class="sxs-lookup"><span data-stu-id="601cc-109">For instructions on how toocreate an HDInsight cluster with Azure Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store as default storage](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).</span></span>

> [!NOTE]
> <span data-ttu-id="601cc-110">Pokud chcete toouse Azure Data Lake Store jako další úložiště pro HDInsight cluster, důrazně doporučujeme, abyste to provedli při vytváření clusteru hello, jak je popsáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="601cc-110">If you are going toouse Azure Data Lake Store as additional storage for HDInsight cluster, we strongly recommend that you do this while you create hello cluster as described in this article.</span></span> <span data-ttu-id="601cc-111">Přidání Azure Data Lake Store jako další úložiště tooan stávající cluster HDInsight je složitý proces a tooerrors náchylné k chybám.</span><span class="sxs-lookup"><span data-stu-id="601cc-111">Adding Azure Data Lake Store as additional storage tooan existing HDInsight cluster is a complicated process and prone tooerrors.</span></span>
>

<span data-ttu-id="601cc-112">Pro typy podporované clusteru slouží jako výchozí úložiště nebo účtu další úložiště Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-112">For supported cluster types, Data Lake Store can be used as a default storage or additional storage account.</span></span> <span data-ttu-id="601cc-113">Pokud se použije jako další úložiště Data Lake Store, hello výchozí účet úložiště pro clustery hello budou mít pořád Azure úložiště objektů BLOB (WASB) a hello clusteru související soubory (například protokoly atd.) se zapisují stále toohello výchozí úložiště, při hello dat, které chtít tooprocess můžou být uložené v účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-113">When Data Lake Store is used as additional storage, hello default storage account for hello clusters will still be Azure Storage Blobs (WASB) and hello cluster-related files (such as logs, etc.) are still written toohello default storage, while hello data that you want tooprocess can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="601cc-114">Pomocí Data Lake Store jako další úložiště účet nemá negativní vliv na výkon nebo hello možnost tooread a zápis toohello úložiště z clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-114">Using Data Lake Store as an additional storage account does not impact performance or hello ability tooread/write toohello storage from hello cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="601cc-115">Pro úložiště clusteru HDInsight pomocí Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="601cc-115">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="601cc-116">Zde jsou některé důležité informace týkající se používání HDInsight s Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="601cc-116">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="601cc-117">Clustery HDInsight možnost toocreate s přístupem k tooData Lake Store jako další úložiště je k dispozici pro HDInsight verze 3.2, 3,4, 3.5 a 3.6.</span><span class="sxs-lookup"><span data-stu-id="601cc-117">Option toocreate HDInsight clusters with access tooData Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="601cc-118">Konfigurace HDInsight s Data Lake Store pomocí prostředí PowerShell toowork zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="601cc-118">Configuring HDInsight toowork with Data Lake Store using PowerShell involves hello following steps:</span></span>

* <span data-ttu-id="601cc-119">Vytvoření Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="601cc-119">Create an Azure Data Lake Store</span></span>
* <span data-ttu-id="601cc-120">Nastavení ověřování pro založené na rolích přístup tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="601cc-120">Set up authentication for role-based access tooData Lake Store</span></span>
* <span data-ttu-id="601cc-121">Vytvoření clusteru HDInsight s ověřování tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="601cc-121">Create HDInsight cluster with authentication tooData Lake Store</span></span>
* <span data-ttu-id="601cc-122">Spustit úlohu testu v clusteru hello</span><span class="sxs-lookup"><span data-stu-id="601cc-122">Run a test job on hello cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="601cc-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="601cc-123">Prerequisites</span></span>
<span data-ttu-id="601cc-124">Než začnete tento kurz, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="601cc-124">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="601cc-125">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="601cc-125">**An Azure subscription**.</span></span> <span data-ttu-id="601cc-126">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="601cc-126">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="601cc-127">**Azure PowerShell 1.0 nebo vyšší**.</span><span class="sxs-lookup"><span data-stu-id="601cc-127">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="601cc-128">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="601cc-128">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="601cc-129">**Windows SDK**.</span><span class="sxs-lookup"><span data-stu-id="601cc-129">**Windows SDK**.</span></span> <span data-ttu-id="601cc-130">Můžete nainstalovat z [zde](https://dev.windows.com/en-us/downloads).</span><span class="sxs-lookup"><span data-stu-id="601cc-130">You can install it from [here](https://dev.windows.com/en-us/downloads).</span></span> <span data-ttu-id="601cc-131">Můžete použít tento toocreate certifikát zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="601cc-131">You use this toocreate a security certificate.</span></span>
* <span data-ttu-id="601cc-132">**Azure Active Directory instanční objekt**.</span><span class="sxs-lookup"><span data-stu-id="601cc-132">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="601cc-133">Kroky v tomto kurzu poskytují pokyny o tom, toocreate objektu služby ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="601cc-133">Steps in this tutorial provide instructions on how toocreate a service principal in Azure AD.</span></span> <span data-ttu-id="601cc-134">Ale musí být webu možné toocreate pro Azure AD správce toobe hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="601cc-134">However, you must be an Azure AD administrator toobe able toocreate a service principal.</span></span> <span data-ttu-id="601cc-135">Pokud jste správce Azure AD, můžete přeskočit tento požadavek a pokračovat v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-135">If you are an Azure AD administrator, you can skip this prerequisite and proceed with hello tutorial.</span></span>

    <span data-ttu-id="601cc-136">**Pokud si nejste správce Azure AD**, nebudete moct tooperform hello kroky požadované toocreate hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="601cc-136">**If you are not an Azure AD administrator**, you will not be able tooperform hello steps required toocreate a service principal.</span></span> <span data-ttu-id="601cc-137">V takovém případě musíte vám správce Azure AD nejdřív vytvořit objekt služby, před vytvořením clusteru HDInsight s Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-137">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="601cc-138">Navíc hello instanční objekt musí být vytvořen pomocí certifikátu, jak je popsáno v [vytvořit objekt služby pomocí certifikátu](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="601cc-138">Also, hello service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-azure-data-lake-store"></a><span data-ttu-id="601cc-139">Vytvoření Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="601cc-139">Create an Azure Data Lake Store</span></span>
<span data-ttu-id="601cc-140">Postupujte podle těchto kroků toocreate Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-140">Follow these steps toocreate a Data Lake Store.</span></span>

1. <span data-ttu-id="601cc-141">Z plochy otevřete nové okno Azure PowerShell a zadejte hello následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="601cc-141">From your desktop, open a new Azure PowerShell window, and enter hello following snippet.</span></span> <span data-ttu-id="601cc-142">Když výzvami toolog, ujistěte se můžete přihlásit jako jeden hello správce/vlastník předplatného:</span><span class="sxs-lookup"><span data-stu-id="601cc-142">When prompted toolog in, make sure you log in as one of hello subscription administrator/owner:</span></span>

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > <span data-ttu-id="601cc-143">Pokud narazíte na chyby, které jsou podobné příliš`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid` při registraci zprostředkovatele prostředků hello Data Lake Store, je možné, že vaše předplatné není povolený pro Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-143">If you receive an error similar too`Register-AzureRmResourceProvider : InvalidResourceNamespace: hello resource namespace 'Microsoft.DataLakeStore' is invalid` when registering hello Data Lake Store resource provider, it is possible that your subscription is not whitelisted for Azure Data Lake Store.</span></span> <span data-ttu-id="601cc-144">Ujistěte se, aktivujte předplatné Azure pro verzi public preview služby Data Lake Store pomocí následujících tyto [pokyny](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="601cc-144">Make sure you enable your Azure subscription for Data Lake Store public preview by following these [instructions](data-lake-store-get-started-portal.md).</span></span>
   >
   >
2. <span data-ttu-id="601cc-145">Účet Azure Data Lake Store je přidružený ke skupině prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="601cc-145">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="601cc-146">Začněte vytvořením skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="601cc-146">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="601cc-147">Měli byste vidět výstup takto:</span><span class="sxs-lookup"><span data-stu-id="601cc-147">You should see an output like this:</span></span>

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. <span data-ttu-id="601cc-148">Vytvořte účet Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-148">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="601cc-149">účet Hello název, který zadáte, musí obsahovat jenom malá písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="601cc-149">hello account name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="601cc-150">Měli byste vidět výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="601cc-150">You should see an output like hello following:</span></span>

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

5. <span data-ttu-id="601cc-151">Nahrajte několik ukázkových dat tooAzure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="601cc-151">Upload some sample data tooAzure Data Lake.</span></span> <span data-ttu-id="601cc-152">Použijeme ho později v tooverify tohoto článku, hello data jsou přístupná z clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="601cc-152">We'll use this later in this article tooverify that hello data is accessible from an HDInsight cluster.</span></span> <span data-ttu-id="601cc-153">Pokud hledáte některé tooupload ukázková data, můžete získat hello **Ambulance Data** složky z hello [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="601cc-153">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path toodata>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-toodata-lake-store"></a><span data-ttu-id="601cc-154">Nastavení ověřování pro založené na rolích přístup tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="601cc-154">Set up authentication for role-based access tooData Lake Store</span></span>
<span data-ttu-id="601cc-155">Každé předplatné služby Azure souvisí s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="601cc-155">Every Azure subscription is associated with an Azure Active Directory.</span></span> <span data-ttu-id="601cc-156">Uživatelů a služeb, která přistupují k prostředkům pomocí hello portálu Azure Classic nebo rozhraní API služby Azure Resource Manager předplatného hello musí nejprve ověřit pomocí této služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="601cc-156">Users and services that access resources of hello subscription using hello Azure Classic Portal or Azure Resource Manager API must first authenticate with that Azure Active Directory.</span></span> <span data-ttu-id="601cc-157">Přístup uděluje přiřazením příslušné role hello na prostředek služby Azure tooAzure odběry a služby.</span><span class="sxs-lookup"><span data-stu-id="601cc-157">Access is granted tooAzure subscriptions and services by assigning them hello appropriate role on an Azure resource.</span></span>  <span data-ttu-id="601cc-158">Objekt služby pro služby, identifikuje hello služby v hello Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="601cc-158">For services, a service principal identifies hello service in hello Azure Active Directory (AAD).</span></span> <span data-ttu-id="601cc-159">Tato část ukazuje, jak toogrant aplikace služby, stejně jako HDInsight, tooan přístup prostředků Azure (hello účet Azure Data Lake Store, které jste vytvořili dříve) vytvořením objekt služby pro aplikaci hello a přiřadit role toothat přes Azure Prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="601cc-159">This section illustrates how toogrant an application service, like HDInsight, access tooan Azure resource (hello Azure Data Lake Store account you created earlier) by creating a service principal for hello application and assigning roles toothat via Azure PowerShell.</span></span>

<span data-ttu-id="601cc-160">tooset až ověřování služby Active Directory pro Azure Data Lake, je nutné provést hello následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="601cc-160">tooset up Active Directory authentication for Azure Data Lake, you must perform hello following tasks.</span></span>

* <span data-ttu-id="601cc-161">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="601cc-161">Create a self-signed certificate</span></span>
* <span data-ttu-id="601cc-162">Vytvoření aplikace v Azure Active Directory a objektu služby</span><span class="sxs-lookup"><span data-stu-id="601cc-162">Create an application in Azure Active Directory and a Service Principal</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="601cc-163">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="601cc-163">Create a self-signed certificate</span></span>
<span data-ttu-id="601cc-164">Zajistěte, aby byla [Windows SDK](https://dev.windows.com/en-us/downloads) nainstalovala ještě před pokračováním hello kroky v této části.</span><span class="sxs-lookup"><span data-stu-id="601cc-164">Make sure you have [Windows SDK](https://dev.windows.com/en-us/downloads) installed before proceeding with hello steps in this section.</span></span> <span data-ttu-id="601cc-165">Musíte také vytvořit adresář, jako například **C:\mycertdir**, kde bude vytvořen hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="601cc-165">You must have also created a directory, such as **C:\mycertdir**, where hello certificate will be created.</span></span>

1. <span data-ttu-id="601cc-166">V okně Powershellu hello přejděte toohello umístění, kam jste nainstalovali Windows SDK (obvykle `C:\Program Files (x86)\Windows Kits\10\bin\x86` a používat hello [MakeCert] [ makecert] nástroj toocreate certifikát podepsaný svým držitelem a privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="601cc-166">From hello PowerShell window, navigate toohello location where you installed Windows SDK (typically, `C:\Program Files (x86)\Windows Kits\10\bin\x86` and use hello [MakeCert][makecert] utility toocreate a self-signed certificate and a private key.</span></span> <span data-ttu-id="601cc-167">Použijte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-167">Use hello following commands.</span></span>

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    <span data-ttu-id="601cc-168">Bude výzvami tooenter hello heslo soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="601cc-168">You will be prompted tooenter hello private key password.</span></span> <span data-ttu-id="601cc-169">Po hello příkaz úspěšně provede, měli byste vidět **CertFile.cer** a **mykey.pvk** v adresáři certifikát hello jste zadali.</span><span class="sxs-lookup"><span data-stu-id="601cc-169">After hello command successfully executes, you should see a **CertFile.cer** and **mykey.pvk** in hello certificate directory you specified.</span></span>
2. <span data-ttu-id="601cc-170">Použití hello [Pvk2Pfx] [ pvk2pfx] nástroj tooconvert hello pvk a .cer soubory tento soubor .pfx vytvořený tooa MakeCert.</span><span class="sxs-lookup"><span data-stu-id="601cc-170">Use hello [Pvk2Pfx][pvk2pfx] utility tooconvert hello .pvk and .cer files that MakeCert created tooa .pfx file.</span></span> <span data-ttu-id="601cc-171">Spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-171">Run hello following command.</span></span>

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    <span data-ttu-id="601cc-172">Po zobrazení výzvy zadejte heslo hello privátní klíče jste dřív zadali.</span><span class="sxs-lookup"><span data-stu-id="601cc-172">When prompted enter hello private key password you specified earlier.</span></span> <span data-ttu-id="601cc-173">Hodnota zadaná pro hello Hello **-SP** parametr je hello heslo, které souvisí s hello soubor .pfx.</span><span class="sxs-lookup"><span data-stu-id="601cc-173">hello value you specify for hello **-po** parameter is hello password that is associated with hello .pfx file.</span></span> <span data-ttu-id="601cc-174">Po úspěšném dokončení příkazu hello, měli byste taky vidět CertFile.pfx v určeném adresáři hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="601cc-174">After hello command successfully completes, you should also see a CertFile.pfx in hello certificate directory you specified.</span></span>

### <a name="create-an-azure-active-directory-and-a-service-principal"></a><span data-ttu-id="601cc-175">Vytvoření služby Azure Active Directory a objektu služby</span><span class="sxs-lookup"><span data-stu-id="601cc-175">Create an Azure Active Directory and a service principal</span></span>
<span data-ttu-id="601cc-176">V této části provádět kroky toocreate hello objekt služby pro aplikaci Azure Active Directory, přiřaďte objekt role toohello služby a ověření jako hello instanční objekt tím, že poskytuje certifikát.</span><span class="sxs-lookup"><span data-stu-id="601cc-176">In this section, you perform hello steps toocreate a service principal for an Azure Active Directory application, assign a role toohello service principal, and authenticate as hello service principal by providing a certificate.</span></span> <span data-ttu-id="601cc-177">Spusťte následující příkazy toocreate hello aplikace v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="601cc-177">Run hello following commands toocreate an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="601cc-178">Vložte následující rutiny v okně konzoly prostředí PowerShell hello hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-178">Paste hello following cmdlets in hello PowerShell console window.</span></span> <span data-ttu-id="601cc-179">Ujistěte se, zda text hello hodnota zadaná pro hello **– DisplayName** vlastnost je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="601cc-179">Make sure hello value you specify for hello **-DisplayName** property is unique.</span></span> <span data-ttu-id="601cc-180">Navíc hello hodnoty pro **– Domovská stránka** a **- IdentiferUris** jsou zástupné hodnoty a nejsou ověřené.</span><span class="sxs-lookup"><span data-stu-id="601cc-180">Also, hello values for **-HomePage** and **-IdentiferUris** are placeholder values and are not verified.</span></span>

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
2. <span data-ttu-id="601cc-181">Vytvořit objekt služby pomocí ID hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="601cc-181">Create a service principal using hello application ID.</span></span>

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. <span data-ttu-id="601cc-182">Udělit hello služby hlavní přístup toohello Data Lake Store složku a soubor hello, kterému bude přístup z clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-182">Grant hello service principal access toohello Data Lake Store folder and hello file that you will access from hello HDInsight cluster.</span></span> <span data-ttu-id="601cc-183">Následující fragment Hello poskytuje přístup toohello kořenovém hello Data Lake Store účtu (které jste zkopírovali hello ukázkový datový soubor) a hello samotném souboru.</span><span class="sxs-lookup"><span data-stu-id="601cc-183">hello snippet below provides access toohello root of hello Data Lake Store account (where you copied hello sample data file), and hello file itself.</span></span>

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a><span data-ttu-id="601cc-184">Vytvoření clusteru služby HDInsight Linux s Data Lake Store jako další úložiště</span><span class="sxs-lookup"><span data-stu-id="601cc-184">Create an HDInsight Linux cluster with Data Lake Store as additional storage</span></span>

<span data-ttu-id="601cc-185">V této části vytvoříme cluster služby HDInsight Hadoop Linux s Data Lake Store jako další úložiště.</span><span class="sxs-lookup"><span data-stu-id="601cc-185">In this section, we create an HDInsight Hadoop Linux cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="601cc-186">Pro tuto verzi clusteru HDInsight hello a hello Data Lake Store musí být v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="601cc-186">For this release, hello HDInsight cluster and hello Data Lake Store must be in hello same location.</span></span>

1. <span data-ttu-id="601cc-187">Začněte s načítání ID hello předplatného klienta.</span><span class="sxs-lookup"><span data-stu-id="601cc-187">Start with retrieving hello subscription tenant ID.</span></span> <span data-ttu-id="601cc-188">Budete potřebovat, který později.</span><span class="sxs-lookup"><span data-stu-id="601cc-188">You will need that later.</span></span>

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. <span data-ttu-id="601cc-189">Pro tuto verzi pro Hadoop cluster, Data Lake Store se použít jenom jako další úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="601cc-189">For this release, for a Hadoop cluster, Data Lake Store can only be used as an additional storage for hello cluster.</span></span> <span data-ttu-id="601cc-190">Hello výchozí úložiště budou mít pořád hello objektů BLOB Azure storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="601cc-190">hello default storage will still be hello Azure storage blobs (WASB).</span></span> <span data-ttu-id="601cc-191">Proto nejdřív vytvoříme hello účet a úložiště kontejnery úložiště potřebné pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="601cc-191">So, we'll first create hello storage account and storage containers required for hello cluster.</span></span>

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. <span data-ttu-id="601cc-192">Vytvoření clusteru HDInsight se hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-192">Create hello HDInsight cluster.</span></span> <span data-ttu-id="601cc-193">Pomocí následující rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-193">Use hello following cmdlets.</span></span>

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have hello same name for hello cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # hello number of nodes in hello HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    <span data-ttu-id="601cc-194">Po úspěšném dokončení hello rutiny, měli byste vidět výstup hello clusteru podrobnosti výpisu.</span><span class="sxs-lookup"><span data-stu-id="601cc-194">After hello cmdlet successfully completes, you should see an output listing hello cluster details.</span></span>


## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a><span data-ttu-id="601cc-195">Spustit testovací úlohy na hello HDInsight clusteru toouse hello Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="601cc-195">Run test jobs on hello HDInsight cluster toouse hello Data Lake Store</span></span>
<span data-ttu-id="601cc-196">Po dokončení konfigurace clusteru služby HDInsight, můžete spustit testovací úlohy na hello clusteru tootest této hello HDInsight clusteru můžete získat přístup k Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-196">After you have configured an HDInsight cluster, you can run test jobs on hello cluster tootest that hello HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="601cc-197">toodo tedy jsme se spustí úlohy Hive ukázka, která vytvoří tabulku pomocí hello ukázková data, který jste nahráli starší tooyour Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-197">toodo so, we will run a sample Hive job that creates a table using hello sample data that you uploaded earlier tooyour Data Lake Store.</span></span>

<span data-ttu-id="601cc-198">V této části se SSH do clusteru HDInsight Linux můžete vytvořit a spustit ukázkový dotaz Hive hello hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-198">In this section you will SSH into hello HDInsight Linux cluster you created and run hello a sample Hive query.</span></span>

* <span data-ttu-id="601cc-199">Pokud používáte tooSSH klienta Windows do hello clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="601cc-199">If you are using a Windows client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="601cc-200">Pokud používáte klienta tooSSH Linux do hello clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="601cc-200">If you are using a Linux client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

1. <span data-ttu-id="601cc-201">Po připojení, spusťte hello Hive rozhraní příkazového řádku pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="601cc-201">Once connected, start hello Hive CLI by using hello following command:</span></span>

        hive
2. <span data-ttu-id="601cc-202">Pomocí hello rozhraní příkazového řádku, zadejte následující příkazy toocreate novou tabulku s názvem hello **vozidel** pomocí hello ukázkových dat v Data Lake Store hello:</span><span class="sxs-lookup"><span data-stu-id="601cc-202">Using hello CLI, enter hello following statements toocreate a new table named **vehicles** by using hello sample data in hello Data Lake Store:</span></span>

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    <span data-ttu-id="601cc-203">Měli byste vidět výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="601cc-203">You should see an output similar toohello following:</span></span>

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1

## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="601cc-204">Přístup k Data Lake Store pomocí příkazů HDFS</span><span class="sxs-lookup"><span data-stu-id="601cc-204">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="601cc-205">Po nakonfigurování hello HDInsight clusteru toouse Data Lake Store můžete hello HDFS prostředí příkazy tooaccess hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="601cc-205">Once you have configured hello HDInsight cluster toouse Data Lake Store, you can use hello HDFS shell commands tooaccess hello store.</span></span>

<span data-ttu-id="601cc-206">V této části se SSH do clusteru HDInsight Linux můžete vytvořit a spustit příkazy HDFS hello hello.</span><span class="sxs-lookup"><span data-stu-id="601cc-206">In this section you will SSH into hello HDInsight Linux cluster you created and run hello HDFS commands.</span></span>

* <span data-ttu-id="601cc-207">Pokud používáte tooSSH klienta Windows do hello clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="601cc-207">If you are using a Windows client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>
* <span data-ttu-id="601cc-208">Pokud používáte klienta tooSSH Linux do hello clusteru, přečtěte si téma [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="601cc-208">If you are using a Linux client tooSSH into hello cluster, see [Use SSH with Linux-based Hadoop on HDInsight from Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

<span data-ttu-id="601cc-209">Po připojení použijte hello následující soubory o hello toolist příkazů systému souborů HDFS v hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-209">Once connected, use hello following HDFS filesystem command toolist hello files in hello Data Lake Store.</span></span>

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

<span data-ttu-id="601cc-210">To by měl seznamu hello souboru, který jste nahráli starší toohello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="601cc-210">This should list hello file that you uploaded earlier toohello Data Lake Store.</span></span>

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

<span data-ttu-id="601cc-211">Můžete taky hello `hdfs dfs -put` příkaz tooupload některé soubory toohello Data Lake Store a pak použijte `hdfs dfs -ls` tooverify jestli hello soubory úspěšně se nahrál.</span><span class="sxs-lookup"><span data-stu-id="601cc-211">You can also use hello `hdfs dfs -put` command tooupload some files toohello Data Lake Store, and then use `hdfs dfs -ls` tooverify whether hello files were successfully uploaded.</span></span>

## <a name="see-also"></a><span data-ttu-id="601cc-212">Viz také</span><span class="sxs-lookup"><span data-stu-id="601cc-212">See Also</span></span>
* [<span data-ttu-id="601cc-213">Portál: Vytvoření toouse clusteru HDInsight Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="601cc-213">Portal: Create an HDInsight cluster toouse Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
