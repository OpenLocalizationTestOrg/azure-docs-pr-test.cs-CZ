---
title: "certifikáty aaaManage v clusteru služby Azure Service Fabric | Microsoft Docs"
description: "Popisuje, jak tooadd nové certifikáty, certifikát výměny a odebrat certifikát tooor z clusteru Service Fabric."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: chackdan
ms.openlocfilehash: 8e57bd95dbb800ecc04cf6988047e3abdc2fe56a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a><span data-ttu-id="c6a8b-103">Přidat nebo odebrat certifikáty pro cluster Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="c6a8b-103">Add or remove certificates for a Service Fabric cluster in Azure</span></span>
<span data-ttu-id="c6a8b-104">Doporučujeme Seznamte se s jak Service Fabric používá certifikáty X.509 a znát hello [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="c6a8b-104">It is recommended that you familiarize yourself with how Service Fabric uses X.509 certificates and be familiar with hello [Cluster security scenarios](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="c6a8b-105">Je potřeba pochopit, jaké certifikát clusteru je a co se používá, před pokračováním.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-105">You must understand what a cluster certificate is and what is used for, before you proceed further.</span></span>

<span data-ttu-id="c6a8b-106">Služby prostředků infrastruktury umožňuje zadat, že dva clusteru certifikáty, primárního a sekundárního, při konfiguraci certifikátů zabezpečení při vytváření clusteru v přidání tooclient certifikáty.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-106">Service fabric lets you specify two cluster certificates, a primary and a secondary, when you configure certificate security during cluster creation, in addition tooclient certificates.</span></span> <span data-ttu-id="c6a8b-107">Odkazovat příliš[vytváření clusteru služby azure přes portál](service-fabric-cluster-creation-via-portal.md) nebo [vytváření clusteru služby azure pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) pro podrobnosti o jejich nastavení na vytvořit čas.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-107">Refer too[creating an azure cluster via portal](service-fabric-cluster-creation-via-portal.md) or [creating an azure cluster via Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) for details on setting them up at create time.</span></span> <span data-ttu-id="c6a8b-108">Pokud zadáte jenom jeden certifikát clusteru na doba pro vytvoření, pak který slouží jako primárního certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-108">If you specify only one cluster certificate at create time, then that is used as hello primary certificate.</span></span> <span data-ttu-id="c6a8b-109">Po vytvoření clusteru můžete přidat nového certifikátu jako sekundární.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-109">After cluster creation, you can add a new certificate as a secondary.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a8b-110">Pro cluster s podporou zabezpečení budete vždy potřebovat alespoň jeden platný clusteru (není odvolaný a ne jeho platnost) certifikát (primární nebo sekundární) nasazené (Pokud ne, přestane hello cluster fungovat).</span><span class="sxs-lookup"><span data-stu-id="c6a8b-110">For a secure cluster, you will always need at least one valid (not revoked and not expired) cluster certificate (primary or secondary) deployed (if not, hello cluster stops functioning).</span></span> <span data-ttu-id="c6a8b-111">90 dní před vypršení platnosti, dosáhnout všechny platné certifikáty hello systém vygeneruje upozornění trasování a také událost stavu upozornění na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-111">90 days before all valid certificates reach expiration, hello system generates a warning trace and also a warning health event on hello node.</span></span> <span data-ttu-id="c6a8b-112">Není aktuálně žádné e-mailu nebo všechna oznámení, která odesílá service fabric, v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-112">There is currently no email or any other notification that service fabric sends out on this topic.</span></span> 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-hello-portal"></a><span data-ttu-id="c6a8b-113">Přidat certifikát sekundární clusteru pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="c6a8b-113">Add a secondary cluster certificate using hello portal</span></span>

<span data-ttu-id="c6a8b-114">Certifikát sekundární clusteru nelze přidat prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-114">Secondary cluster certificate cannot be added through hello Azure portal.</span></span> <span data-ttu-id="c6a8b-115">Máte toouse Azure powershell, pro který.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-115">You have toouse Azure powershell for that.</span></span> <span data-ttu-id="c6a8b-116">proces Hello popsané dál v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-116">hello process is outlined later in this document.</span></span>

## <a name="swap-hello-cluster-certificates-using-hello-portal"></a><span data-ttu-id="c6a8b-117">Vyměnit certifikáty hello clusteru pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="c6a8b-117">Swap hello cluster certificates using hello portal</span></span>

<span data-ttu-id="c6a8b-118">Po úspěšně jste nasadili certifikát sekundární clusteru, pokud chcete tooswap hello primárního a sekundárního, pak přejděte okno toohello zabezpečení a vyberte možnost "Prohození s primární" hello z hello kontextové nabídky tooswap hello sekundární certifikátu s primární cert Hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-118">After you have successfully deployed a secondary cluster certificate, if you want tooswap hello primary and secondary, then navigate toohello Security blade, and select hello 'Swap with primary' option from hello context menu tooswap hello secondary cert with hello primary cert.</span></span>

![Swap certifikátu][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-hello-portal"></a><span data-ttu-id="c6a8b-120">Odebrat certifikát clusteru pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="c6a8b-120">Remove a cluster certificate using hello portal</span></span>

<span data-ttu-id="c6a8b-121">Pro cluster s podporou zabezpečení budete vždy potřebovat alespoň jeden platný (není odvolaný a ne jeho platnost) certifikát (primární nebo sekundární) nasazené v opačném případě hello clusteru přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-121">For a secure cluster, you will always need at least one valid (not revoked and not expired) certificate (primary or secondary) deployed if not, hello cluster stops functioning.</span></span>

<span data-ttu-id="c6a8b-122">tooremove sekundární certifikát se používá k zabezpečení clusteru, přejděte toohello zabezpečení okno a vyberte hello, odstranit, možnost hello místní nabídce na sekundární certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-122">tooremove a secondary certificate from being used for cluster security, Navigate toohello Security blade and select hello 'Delete' option from hello context menu on hello secondary certificate.</span></span>

<span data-ttu-id="c6a8b-123">Pokud vaše záměrem tooremove hello certifikát, který je označen jako primární, bude nutné tooswap její hello sekundární nejprve a pak odstraňte hello sekundární po dokončení upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-123">If your intent is tooremove hello certificate that is marked primary, then you will need tooswap it with hello secondary first, and then delete hello secondary after hello upgrade has completed.</span></span>

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a><span data-ttu-id="c6a8b-124">Přidat sekundární certifikát pomocí Správce prostředků Powershell</span><span class="sxs-lookup"><span data-stu-id="c6a8b-124">Add a secondary certificate using Resource Manager Powershell</span></span>

<span data-ttu-id="c6a8b-125">Tento postup předpokládá se seznámíte s fungování Resource Manager a nasadili alespoň jeden pomocí šablony Resource Manageru cluster Service Fabric a hello šablona, kterou jste použili tooset až hello clusteru užitečné.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-125">These steps assume that you are familiar with how Resource Manager works and have deployed atleast one Service Fabric cluster using a Resource Manager template, and have hello template that you used tooset up hello cluster handy.</span></span> <span data-ttu-id="c6a8b-126">Taky se předpokládá, že umíte pomocí JSON.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-126">It is also assumed that you are comfortable using JSON.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a8b-127">Pokud hledáte vzorové šablony a parametry, které můžete použít toofollow společně nebo jako výchozí bod, poté ji stáhnout z tohoto [úložiště git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="c6a8b-127">If you are looking for a sample template and parameters that you can use toofollow along or as a starting point, then download it from this [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span> 
> 
> 

### <a name="edit-your-resource-manager-template"></a><span data-ttu-id="c6a8b-128">Upravte svou šablonu Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c6a8b-128">Edit your Resource Manager template</span></span>

<span data-ttu-id="c6a8b-129">Pro usnadnění následující společně obsahuje ukázkové 5-VM-1-NodeTypes-Secure_Step2.JSON všechny hello úpravy, které budeme provádět.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-129">For ease of following along, sample 5-VM-1-NodeTypes-Secure_Step2.JSON contains all hello edits we will be making.</span></span> <span data-ttu-id="c6a8b-130">Hello ukázka je dostupná v [úložiště git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="c6a8b-130">hello sample is available at [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span>

<span data-ttu-id="c6a8b-131">**Ujistěte se, že toofollow všechny kroky hello**</span><span class="sxs-lookup"><span data-stu-id="c6a8b-131">**Make sure toofollow all hello steps**</span></span>

<span data-ttu-id="c6a8b-132">**Krok 1:** otevřete šablony Resource Manageru hello používá toodeploy můžete clusteru.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-132">**Step 1:** Open up hello Resource Manager template you used toodeploy you Cluster.</span></span> <span data-ttu-id="c6a8b-133">(Pokud jste si stáhli hello ukázka z hello výše úložiště, pak použít 5-VM-1-NodeTypes-Secure_Step1.JSON toodeploy zabezpečení clusteru a pak otevřete si tato šablona).</span><span class="sxs-lookup"><span data-stu-id="c6a8b-133">(If you have downloaded hello sample from hello above repo, then Use 5-VM-1-NodeTypes-Secure_Step1.JSON toodeploy a secure cluster and then open up that template).</span></span>

<span data-ttu-id="c6a8b-134">**Krok 2:** přidat **dva nové parametry** "secCertificateThumbprint" a "secCertificateUrlValue" typu "string" toohello části parametr šablony.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-134">**Step 2:** Add **two new parameters** "secCertificateThumbprint" and "secCertificateUrlValue" of type "string" toohello parameter section of your template.</span></span> <span data-ttu-id="c6a8b-135">Můžete zkopírovat hello následující fragment kódu a přidejte ho toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-135">You can copy hello following code snippet and add it toohello template.</span></span> <span data-ttu-id="c6a8b-136">V závislosti na hello zdroj šablony mohou už tyto definované, pokud ano, přesuňte toohello další krok.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-136">Depending on hello source of your template, you may already have these defined, if so move toohello next step.</span></span> 
 
```JSON
   "secCertificateThumbprint": {
      "type": "string",
      "metadata": {
        "description": "Certificate Thumbprint"
      }
    },
    "secCertificateUrlValue": {
      "type": "string",
      "metadata": {
        "description": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
      }
    },

```

<span data-ttu-id="c6a8b-137">**Krok 3:** provádět změny toohello **Microsoft.ServiceFabric/clusters** prostředku, vyhledejte definice prostředků hello "Microsoft.ServiceFabric/clusters" v šabloně.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-137">**Step 3:** Make changes toohello **Microsoft.ServiceFabric/clusters** resource - Locate hello "Microsoft.ServiceFabric/clusters" resource definition in your template.</span></span> <span data-ttu-id="c6a8b-138">V části Vlastnosti definice zjistíte "Certifikát" JSON značku, která by měla vypadat podobně jako hello následujícím fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="c6a8b-138">Under properties of that definition, you will find "Certificate" JSON tag, which should look something like hello following JSON snippet:</span></span>

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="c6a8b-139">Přidat novou značku "thumbprintSecondary" a dejte mu hodnotu "[parameters('secCertificateThumbprint')]".</span><span class="sxs-lookup"><span data-stu-id="c6a8b-139">Add a new tag "thumbprintSecondary" and give it a value "[parameters('secCertificateThumbprint')]".</span></span>  

<span data-ttu-id="c6a8b-140">Ano, teď hello definice prostředků by měl vypadat jako následující hello (v závislosti na vaší zdroje hello šablony, nemusí být úplně stejně jako hello fragment kódu níže).</span><span class="sxs-lookup"><span data-stu-id="c6a8b-140">So now hello resource definition should look like hello following (depending on your source of hello template, it may not be exactly like hello snippet below).</span></span> 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="c6a8b-141">Pokud chcete příliš**výměny hello cert**, pak zadejte hello nového certifikátu jako primární a přesunutí hello aktuální primární jako sekundární.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-141">If you want too**rollover hello cert**, then specify hello new cert as primary and moving hello current primary as secondary.</span></span> <span data-ttu-id="c6a8b-142">Výsledkem hello výměna vaše aktuální primární certifikát toohello nový certifikát v jednom kroku nasazení.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-142">This results in hello rollover of your current primary certificate toohello new certificate in one deployment step.</span></span>

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


<span data-ttu-id="c6a8b-143">**Krok 4:** provádět změny příliš**všechny** hello **Microsoft.Compute/virtualMachineScaleSets** definice prostředků - vyhledání prostředků Microsoft.Compute/virtualMachineScaleSets hello definice.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-143">**Step 4:** Make changes too**all** hello **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate hello Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="c6a8b-144">Posuňte se toohello "vydavatel": "Microsoft.Azure.ServiceFabric" v části "virtualMachineProfile".</span><span class="sxs-lookup"><span data-stu-id="c6a8b-144">Scroll toohello "publisher": "Microsoft.Azure.ServiceFabric", under "virtualMachineProfile".</span></span>

<span data-ttu-id="c6a8b-145">V nastavení hello služby fabric vydavatele měli byste vidět zhruba takhle.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-145">In hello service fabric publisher settings, you should see something like this.</span></span>

![Json_Pub_Setting1][Json_Pub_Setting1]

<span data-ttu-id="c6a8b-147">Přidat hello tooit položky nového certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-147">Add hello new cert entries tooit</span></span>

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

<span data-ttu-id="c6a8b-148">Vlastnosti Hello by teď měl vypadat takto</span><span class="sxs-lookup"><span data-stu-id="c6a8b-148">hello properties should now look like this</span></span>

![Json_Pub_Setting2][Json_Pub_Setting2]

<span data-ttu-id="c6a8b-150">Pokud chcete příliš**výměny hello cert**, pak zadejte hello nového certifikátu jako primární a přesunutí hello aktuální primární jako sekundární.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-150">If you want too**rollover hello cert**, then specify hello new cert as primary and moving hello current primary as secondary.</span></span> <span data-ttu-id="c6a8b-151">Výsledkem hello výměna vaše aktuální certifikát toohello nový certifikát v jednom kroku nasazení.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-151">This results in hello rollover of your current certificate toohello new certificate in one deployment step.</span></span> 


```JSON
               "certificate": {
                   "thumbprint": "[parameters('secCertificateThumbprint')]",
                   "x509StoreName": "[parameters('certificateStoreValue')]"
                     },
               "certificateSecondary": {
                    "thumbprint": "[parameters('certificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```
<span data-ttu-id="c6a8b-152">Vlastnosti Hello by teď měl vypadat takto</span><span class="sxs-lookup"><span data-stu-id="c6a8b-152">hello properties should now look like this</span></span>

![Json_Pub_Setting3][Json_Pub_Setting3]


<span data-ttu-id="c6a8b-154">**Krok 5:** změnit příliš**všechny** hello **Microsoft.Compute/virtualMachineScaleSets** definice prostředků - vyhledání prostředků Microsoft.Compute/virtualMachineScaleSets hello definice.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-154">**Step 5:** Make Changes too**all** hello **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate hello Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="c6a8b-155">Posuňte se toohello "vaultCertificates":, v části "OSProfile".</span><span class="sxs-lookup"><span data-stu-id="c6a8b-155">Scroll toohello "vaultCertificates": , under "OSProfile".</span></span> <span data-ttu-id="c6a8b-156">by měla vypadat přibližně takto.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-156">it should look something like this.</span></span>


![Json_Pub_Setting4][Json_Pub_Setting4]

<span data-ttu-id="c6a8b-158">Přidejte hello secCertificateUrlValue tooit.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-158">Add hello secCertificateUrlValue tooit.</span></span> <span data-ttu-id="c6a8b-159">Použijte hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="c6a8b-159">use hello following snippet:</span></span>

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
<span data-ttu-id="c6a8b-160">Nyní hello výsledná Json by měla vypadat přibližně takto.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-160">Now hello resulting Json should look something like this.</span></span>
<span data-ttu-id="c6a8b-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span><span class="sxs-lookup"><span data-stu-id="c6a8b-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span></span>


> [!NOTE]
> <span data-ttu-id="c6a8b-162">Ujistěte se, že obsahovat opakované kroky 4 a 5 pro všechny definice prostředků Nodetypes/Microsoft.Compute/virtualMachineScaleSets hello ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-162">Make sure that you have repeated steps 4 and 5 for all hello Nodetypes/Microsoft.Compute/virtualMachineScaleSets resource definitions in your template.</span></span> <span data-ttu-id="c6a8b-163">Pokud jeden z nich přeskočíte, nebude získat hello certifikát nainstalovaný na tomto VMSS a budete mít vést k neočekávaným výsledkům v clusteru, včetně hello clusteru směrem dolů (Pokud skončili žádné platné certifikáty, že Hello clusteru můžete použít pro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-163">If you miss one of them, hello certificate will not get installed on that VMSS and you will have unpredictable results in your cluster, including hello cluster going down (if you end up with no valid certificates that hello cluster can use for security.</span></span> <span data-ttu-id="c6a8b-164">Proto prosím Překontrolujte, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-164">So please double check, before proceeding further.</span></span>
> 
> 


### <a name="edit-your-template-file-tooreflect-hello-new-parameters-you-added-above"></a><span data-ttu-id="c6a8b-165">Upravit šablonu soubor tooreflect hello nové parametry, které jste přidali výše</span><span class="sxs-lookup"><span data-stu-id="c6a8b-165">Edit your template file tooreflect hello new parameters you added above</span></span>
<span data-ttu-id="c6a8b-166">Pokud používáte ukázku hello z hello [úložiště git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) toofollow hotová, můžete spustit toomake změny v hello ukázka 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON</span><span class="sxs-lookup"><span data-stu-id="c6a8b-166">If you are using hello sample from hello [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) toofollow along, you can start toomake changes in hello sample 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON</span></span> 

<span data-ttu-id="c6a8b-167">Upravit daný parametr šablony Resource Manageru soubor, přidejte dva nové parametry pro secCertificateThumbprint a secCertificateUrlValue hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-167">Edit your Resource Manager Template parameter File, add hello two new parameters for secCertificateThumbprint and secCertificateUrlValue.</span></span> 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-hello-template-tooazure"></a><span data-ttu-id="c6a8b-168">Nasazení šablony tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="c6a8b-168">Deploy hello template tooAzure</span></span>

- <span data-ttu-id="c6a8b-169">Můžete je nyní připraven toodeploy tooAzure vaší šablony.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-169">You are now ready toodeploy your template tooAzure.</span></span> <span data-ttu-id="c6a8b-170">Otevřete příkazový řádek se verze 1 + Azure PS.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-170">Open an Azure PS version 1+ command prompt.</span></span>
- <span data-ttu-id="c6a8b-171">Přihlaste tooyour účet Azure a vybrat konkrétní předplatné azure hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-171">Log in tooyour Azure Account and select hello specific azure subscription.</span></span> <span data-ttu-id="c6a8b-172">Toto je důležitý krok pro zaměstnance, kteří mají přístup toomore než jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-172">This is an important step for folks who have access toomore than one azure subscription.</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

<span data-ttu-id="c6a8b-173">Testování hello šablony předchozí toodeploying ho.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-173">Test hello template prior toodeploying it.</span></span> <span data-ttu-id="c6a8b-174">Použití hello stejnou skupinu prostředků, který váš cluster je aktuálně nasazený do.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-174">Use hello same Resource Group that your cluster is currently deployed to.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

<span data-ttu-id="c6a8b-175">Nasazení skupiny prostředků tooyour hello šablony.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-175">Deploy hello template tooyour resource group.</span></span> <span data-ttu-id="c6a8b-176">Použití hello stejnou skupinu prostředků, který váš cluster je aktuálně nasazený do.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-176">Use hello same Resource Group that your cluster is currently deployed to.</span></span> <span data-ttu-id="c6a8b-177">Spuštěním příkazu New-AzureRmResourceGroupDeployment hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-177">Run hello New-AzureRmResourceGroupDeployment command.</span></span> <span data-ttu-id="c6a8b-178">Není nutné toospecify hello režimu, protože hello výchozí hodnota je **přírůstkové**.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-178">You do not need toospecify hello mode, since hello default value is **incremental**.</span></span>

> [!NOTE]
> <span data-ttu-id="c6a8b-179">Pokud jste nastavili tooComplete režimu, můžete nechtěně odstranit prostředky, které nejsou ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-179">If you set Mode tooComplete, you can inadvertently delete resources that are not in your template.</span></span> <span data-ttu-id="c6a8b-180">Nepoužívejte ho v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-180">So do not use it in this scenario.</span></span>
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

<span data-ttu-id="c6a8b-181">Tady je vyplnění si příklad hello stejné prostředí powershell.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-181">Here is a filled out example of hello same powershell.</span></span>

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

<span data-ttu-id="c6a8b-182">Po dokončení nasazení hello připojit pomocí clusteru tooyour hello nový certifikát a provádět některé dotazy.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-182">Once hello deployment is complete, connect tooyour cluster using hello new Certificate and perform some queries.</span></span> <span data-ttu-id="c6a8b-183">Pokud jste možnost toodo.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-183">If you are able toodo.</span></span> <span data-ttu-id="c6a8b-184">Potom můžete odstranit hello starý certifikát.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-184">Then you can delete hello old certificate.</span></span> 

<span data-ttu-id="c6a8b-185">Pokud používáte certifikát podepsaný svým držitelem, nevynechali tooimport je do místního úložiště certifikátů TrustedPeople.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-185">If you are using a self-signed certificate, do not forget tooimport them into your local TrustedPeople cert store.</span></span>

```powershell
######## Set up hello certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
<span data-ttu-id="c6a8b-186">Pro rychlou referenci tady je hello příkaz tooconnect tooa zabezpečení clusteru</span><span class="sxs-lookup"><span data-stu-id="c6a8b-186">For quick reference here is hello command tooconnect tooa secure cluster</span></span> 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
<span data-ttu-id="c6a8b-187">Pro rychlou referenci tady je hello příkaz tooget clusteru stavu</span><span class="sxs-lookup"><span data-stu-id="c6a8b-187">For quick reference here is hello command tooget cluster health</span></span>

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-toohello-cluster"></a><span data-ttu-id="c6a8b-188">Nasazení clusteru toohello certifikáty aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-188">Deploying Application certificates toohello cluster.</span></span>

<span data-ttu-id="c6a8b-189">Můžete použít stejné kroky, jak je uvedeno v kroky 5 výše toohave hello certifikáty z keyvault toohello uzly nasazena hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-189">You can use hello same steps as outlined in Steps 5 above toohave hello certificates deployed from a keyvault toohello Nodes.</span></span> <span data-ttu-id="c6a8b-190">Stačí nutné definovat a použít jiné parametry.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-190">you just need define and use different parameters.</span></span>


## <a name="adding-or-removing-client-certificates"></a><span data-ttu-id="c6a8b-191">Přidání nebo odebrání klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="c6a8b-191">Adding or removing Client certificates</span></span>

<span data-ttu-id="c6a8b-192">V modulu snap-in Certifikáty clusteru toohello přidání můžete přidat klientské certifikáty tooperform management operace na service fabric cluster.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-192">In addition toohello cluster certificates, you can add client certificates tooperform management operations on a service fabric cluster.</span></span>

<span data-ttu-id="c6a8b-193">Můžete přidat dva druhy klientské certifikáty - správce nebo jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-193">You can add two kinds of client certificates - Admin or Read-only.</span></span> <span data-ttu-id="c6a8b-194">To poté mohou být operace správce toohello použité toocontrol přístup a operace dotazů na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-194">These then can be used toocontrol access toohello admin operations and Query operations on hello cluster.</span></span> <span data-ttu-id="c6a8b-195">Ve výchozím nastavení certifikáty clusteru hello se přidají toohello správce certifikátů seznamu povolených aplikací.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-195">By default, hello cluster certificates are added toohello allowed Admin certificates list.</span></span>

<span data-ttu-id="c6a8b-196">můžete zadat libovolný počet klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-196">you can specify any number of client certificates.</span></span> <span data-ttu-id="c6a8b-197">Každý přidávání a odstraňování výsledkem cluster konfigurace aktualizace toohello service fabric</span><span class="sxs-lookup"><span data-stu-id="c6a8b-197">Each addition/deletion results in a configuration update toohello service fabric cluster</span></span>


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a><span data-ttu-id="c6a8b-198">Přidání klientské certifikáty - správce nebo jen pro čtení přes portál</span><span class="sxs-lookup"><span data-stu-id="c6a8b-198">Adding client certificates - Admin or Read-Only via portal</span></span>

1. <span data-ttu-id="c6a8b-199">Přejděte okno toohello zabezpečení a vyberte hello '+ ověřování' tlačítka v okně zabezpečení hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-199">Navigate toohello Security blade, and select hello '+ Authentication' button on top of hello security blade.</span></span>
2. <span data-ttu-id="c6a8b-200">V okně hello "přidat ověřování zvolte hello"Ověřování typu"- 'jen pro čtení klienta' nebo 'správce klienta.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-200">On hello 'Add Authentication' blade, choose hello 'Authentication Type' - 'Read-only client' or 'Admin client'</span></span>
3. <span data-ttu-id="c6a8b-201">Teď zvolte metoda autorizace hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-201">Now choose hello Authorization method.</span></span> <span data-ttu-id="c6a8b-202">To znamená tooService prostředků infrastruktury, zda se má tento certifikát vyhledávání na základě hello název subjektu nebo miniaturu hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-202">This indicates tooService Fabric whether it should look up this certificate by using hello subject name or hello thumbprint.</span></span> <span data-ttu-id="c6a8b-203">Obecně platí není dobře zabezpečená hello toouse postupem autorizační metoda název subjektu.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-203">In general, it is not a good security practice toouse hello authorization method of subject name.</span></span> 

![Přidání certifikátu klienta][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-hello-portal"></a><span data-ttu-id="c6a8b-205">Odstranění klientské certifikáty - správce nebo jen pro čtení pomocí hello portálu</span><span class="sxs-lookup"><span data-stu-id="c6a8b-205">Deletion of Client Certificates - Admin or Read-Only using hello portal</span></span>

<span data-ttu-id="c6a8b-206">tooremove sekundární certifikát od používá pro zabezpečení clusteru, přejděte toohello zabezpečení okno a vyberte hello 'Delete' z kontextové nabídky hello na hello konkrétní certifikát.</span><span class="sxs-lookup"><span data-stu-id="c6a8b-206">tooremove a secondary certificate from being used for cluster security, Navigate toohello Security blade and select hello 'Delete' option from hello context menu on hello specific certificate.</span></span>



## <a name="next-steps"></a><span data-ttu-id="c6a8b-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6a8b-207">Next steps</span></span>
<span data-ttu-id="c6a8b-208">Přečtěte si tyto články pro další informace o správě clusteru:</span><span class="sxs-lookup"><span data-stu-id="c6a8b-208">Read these articles for more information on cluster management:</span></span>

* [<span data-ttu-id="c6a8b-209">Proces upgradu Service Fabric Cluster a očekávání od vás</span><span class="sxs-lookup"><span data-stu-id="c6a8b-209">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="c6a8b-210">Instalační program přístup na základě rolí pro klienty</span><span class="sxs-lookup"><span data-stu-id="c6a8b-210">Setup role-based access for clients</span></span>](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


