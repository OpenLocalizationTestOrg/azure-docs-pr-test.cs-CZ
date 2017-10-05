---
title: "Správa certifikátů v clusteru služby Azure Service Fabric | Microsoft Docs"
description: "Popisuje, jak přidat nové certifikáty, certifikát výměny a odebrat certifikát do nebo z clusteru Service Fabric."
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
ms.openlocfilehash: c433e8683755e454f9561f094269c3daccf78a62
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a><span data-ttu-id="f28de-103">Přidat nebo odebrat certifikáty pro cluster Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="f28de-103">Add or remove certificates for a Service Fabric cluster in Azure</span></span>
<span data-ttu-id="f28de-104">Doporučujeme Seznamte se s jak Service Fabric používá certifikáty X.509 a znát [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="f28de-104">It is recommended that you familiarize yourself with how Service Fabric uses X.509 certificates and be familiar with the [Cluster security scenarios](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="f28de-105">Je potřeba pochopit, jaké certifikát clusteru je a co se používá, před pokračováním.</span><span class="sxs-lookup"><span data-stu-id="f28de-105">You must understand what a cluster certificate is and what is used for, before you proceed further.</span></span>

<span data-ttu-id="f28de-106">Service fabric umožňuje zadat dva certifikáty clusteru, primárního a sekundárního, když konfigurujete certifikát zabezpečení při vytváření clusteru, kromě klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="f28de-106">Service fabric lets you specify two cluster certificates, a primary and a secondary, when you configure certificate security during cluster creation, in addition to client certificates.</span></span> <span data-ttu-id="f28de-107">Odkazovat na [vytváření clusteru služby azure přes portál](service-fabric-cluster-creation-via-portal.md) nebo [vytváření clusteru služby azure pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) pro podrobnosti o jejich nastavení na vytvořit čas.</span><span class="sxs-lookup"><span data-stu-id="f28de-107">Refer to [creating an azure cluster via portal](service-fabric-cluster-creation-via-portal.md) or [creating an azure cluster via Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) for details on setting them up at create time.</span></span> <span data-ttu-id="f28de-108">Pokud zadáte jenom jeden certifikát clusteru na doba pro vytvoření, pak, který se používá jako primární certifikát.</span><span class="sxs-lookup"><span data-stu-id="f28de-108">If you specify only one cluster certificate at create time, then that is used as the primary certificate.</span></span> <span data-ttu-id="f28de-109">Po vytvoření clusteru můžete přidat nového certifikátu jako sekundární.</span><span class="sxs-lookup"><span data-stu-id="f28de-109">After cluster creation, you can add a new certificate as a secondary.</span></span>

> [!NOTE]
> <span data-ttu-id="f28de-110">Pro cluster s podporou zabezpečení je vždy nutné alespoň jeden platný clusteru (není odvolaný a ne jeho platnost) certifikátu (primární nebo sekundární) nasazeného (Pokud ne, přestane cluster fungovat).</span><span class="sxs-lookup"><span data-stu-id="f28de-110">For a secure cluster, you will always need at least one valid (not revoked and not expired) cluster certificate (primary or secondary) deployed (if not, the cluster stops functioning).</span></span> <span data-ttu-id="f28de-111">90 dní před všechny platné certifikáty dosáhnout vypršení platnosti, systém vygeneruje upozornění trasování a také událost stavu upozornění na uzlu.</span><span class="sxs-lookup"><span data-stu-id="f28de-111">90 days before all valid certificates reach expiration, the system generates a warning trace and also a warning health event on the node.</span></span> <span data-ttu-id="f28de-112">Není aktuálně žádné e-mailu nebo všechna oznámení, která odesílá service fabric, v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="f28de-112">There is currently no email or any other notification that service fabric sends out on this topic.</span></span> 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-the-portal"></a><span data-ttu-id="f28de-113">Přidat certifikát sekundární clusteru pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="f28de-113">Add a secondary cluster certificate using the portal</span></span>

<span data-ttu-id="f28de-114">Nelze přidat certifikát sekundární clusteru prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f28de-114">Secondary cluster certificate cannot be added through the Azure portal.</span></span> <span data-ttu-id="f28de-115">Budete muset použít Azure powershell pro tento.</span><span class="sxs-lookup"><span data-stu-id="f28de-115">You have to use Azure powershell for that.</span></span> <span data-ttu-id="f28de-116">Proces popsané dál v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="f28de-116">The process is outlined later in this document.</span></span>

## <a name="swap-the-cluster-certificates-using-the-portal"></a><span data-ttu-id="f28de-117">Vyměnit certifikáty clusteru pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="f28de-117">Swap the cluster certificates using the portal</span></span>

<span data-ttu-id="f28de-118">Po úspěšně jste nasadili certifikát sekundární clusteru, pokud chcete Prohodit primární a sekundární, přejděte do okna zabezpečení a vyberte možnost 'Prohození s primární' v místní nabídce se Prohodit sekundární certifikátu s primární certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f28de-118">After you have successfully deployed a secondary cluster certificate, if you want to swap the primary and secondary, then navigate to the Security blade, and select the 'Swap with primary' option from the context menu to swap the secondary cert with the primary cert.</span></span>

![Swap certifikátu][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-the-portal"></a><span data-ttu-id="f28de-120">Odebrat certifikát clusteru pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="f28de-120">Remove a cluster certificate using the portal</span></span>

<span data-ttu-id="f28de-121">Pro cluster s podporou zabezpečení budete vždy potřebovat alespoň jeden platný (není odvolaný a ne jeho platnost) certifikát (primární nebo sekundární) nasazené Pokud ne, přestane cluster fungovat.</span><span class="sxs-lookup"><span data-stu-id="f28de-121">For a secure cluster, you will always need at least one valid (not revoked and not expired) certificate (primary or secondary) deployed if not, the cluster stops functioning.</span></span>

<span data-ttu-id="f28de-122">K odebrání sekundární certifikátu používá pro zabezpečení clusteru, přejděte do okna zabezpečení a vyberte možnost 'Delete' z kontextové nabídky na sekundární certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f28de-122">To remove a secondary certificate from being used for cluster security, Navigate to the Security blade and select the 'Delete' option from the context menu on the secondary certificate.</span></span>

<span data-ttu-id="f28de-123">Pokud vaše záměrem odebrat certifikát, který je označen jako primární, pak budete muset Prohodit s sekundární a potom odstraňte sekundární po dokončení upgradu.</span><span class="sxs-lookup"><span data-stu-id="f28de-123">If your intent is to remove the certificate that is marked primary, then you will need to swap it with the secondary first, and then delete the secondary after the upgrade has completed.</span></span>

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a><span data-ttu-id="f28de-124">Přidat sekundární certifikát pomocí Správce prostředků Powershell</span><span class="sxs-lookup"><span data-stu-id="f28de-124">Add a secondary certificate using Resource Manager Powershell</span></span>

<span data-ttu-id="f28de-125">Tento postup předpokládá se seznámíte s fungování Resource Manager a nasadili alespoň jeden pomocí šablony Resource Manageru cluster Service Fabric a mít šablony, která jste použili k nastavení užitečné clusteru.</span><span class="sxs-lookup"><span data-stu-id="f28de-125">These steps assume that you are familiar with how Resource Manager works and have deployed atleast one Service Fabric cluster using a Resource Manager template, and have the template that you used to set up the cluster handy.</span></span> <span data-ttu-id="f28de-126">Taky se předpokládá, že umíte pomocí JSON.</span><span class="sxs-lookup"><span data-stu-id="f28de-126">It is also assumed that you are comfortable using JSON.</span></span>

> [!NOTE]
> <span data-ttu-id="f28de-127">Pokud hledáte vzorové šablony a parametry, které můžete použít postup popsaný společně nebo jako výchozí bod, poté ji stáhnout z tohoto [úložiště git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="f28de-127">If you are looking for a sample template and parameters that you can use to follow along or as a starting point, then download it from this [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span> 
> 
> 

### <a name="edit-your-resource-manager-template"></a><span data-ttu-id="f28de-128">Upravte svou šablonu Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f28de-128">Edit your Resource Manager template</span></span>

<span data-ttu-id="f28de-129">Pro usnadnění následující společně obsahuje ukázkové 5-VM-1-NodeTypes-Secure_Step2.JSON všechny úpravy, které budeme provádět.</span><span class="sxs-lookup"><span data-stu-id="f28de-129">For ease of following along, sample 5-VM-1-NodeTypes-Secure_Step2.JSON contains all the edits we will be making.</span></span> <span data-ttu-id="f28de-130">Ukázka je dostupná v [úložiště git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="f28de-130">the sample is available at [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span>

<span data-ttu-id="f28de-131">**Zajistěte, aby k provedení všech kroků**</span><span class="sxs-lookup"><span data-stu-id="f28de-131">**Make sure to follow all the steps**</span></span>

<span data-ttu-id="f28de-132">**Krok 1:** otevřete šablony Resource Manageru můžete použít k nasazení můžete clusteru.</span><span class="sxs-lookup"><span data-stu-id="f28de-132">**Step 1:** Open up the Resource Manager template you used to deploy you Cluster.</span></span> <span data-ttu-id="f28de-133">(Pokud jste si stáhli ukázky z výše uvedených úložiště, pak 5-VM-1-NodeTypes-Secure_Step1.JSON použijte k nasazení clusteru s podporou zabezpečení a potom otevře tato šablona).</span><span class="sxs-lookup"><span data-stu-id="f28de-133">(If you have downloaded the sample from the above repo, then Use 5-VM-1-NodeTypes-Secure_Step1.JSON to deploy a secure cluster and then open up that template).</span></span>

<span data-ttu-id="f28de-134">**Krok 2:** přidat **dva nové parametry** "secCertificateThumbprint" a "secCertificateUrlValue" z typu "řetězec" do části parametr šablony.</span><span class="sxs-lookup"><span data-stu-id="f28de-134">**Step 2:** Add **two new parameters** "secCertificateThumbprint" and "secCertificateUrlValue" of type "string" to the parameter section of your template.</span></span> <span data-ttu-id="f28de-135">Můžete zkopírovat následující fragment kódu a přidání do šablony.</span><span class="sxs-lookup"><span data-stu-id="f28de-135">You can copy the following code snippet and add it to the template.</span></span> <span data-ttu-id="f28de-136">V závislosti na zdroji šablony už můžete mít tyto definována, pokud tak přesunout k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="f28de-136">Depending on the source of your template, you may already have these defined, if so move to the next step.</span></span> 
 
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
        "description": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
      }
    },

```

<span data-ttu-id="f28de-137">**Krok 3:** provádět změny **Microsoft.ServiceFabric/clusters** prostředku, vyhledejte "Microsoft.ServiceFabric/clusters" definice prostředků ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="f28de-137">**Step 3:** Make changes to the **Microsoft.ServiceFabric/clusters** resource - Locate the "Microsoft.ServiceFabric/clusters" resource definition in your template.</span></span> <span data-ttu-id="f28de-138">V části Vlastnosti definice zjistíte "Certifikát" JSON značku, která by měla vypadat podobně jako následujícím fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="f28de-138">Under properties of that definition, you will find "Certificate" JSON tag, which should look something like the following JSON snippet:</span></span>

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="f28de-139">Přidat novou značku "thumbprintSecondary" a dejte mu hodnotu "[parameters('secCertificateThumbprint')]".</span><span class="sxs-lookup"><span data-stu-id="f28de-139">Add a new tag "thumbprintSecondary" and give it a value "[parameters('secCertificateThumbprint')]".</span></span>  

<span data-ttu-id="f28de-140">Ano, teď by měl vypadat jako následující definici prostředků (v závislosti na vaší zdrojové šablony, nemusí být úplně stejně jako na následujícím fragmentu).</span><span class="sxs-lookup"><span data-stu-id="f28de-140">So now the resource definition should look like the following (depending on your source of the template, it may not be exactly like the snippet below).</span></span> 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="f28de-141">Pokud chcete **výměny certifikát**, zadejte nový certifikát jako primární a přesunutí aktuální primární jako sekundární.</span><span class="sxs-lookup"><span data-stu-id="f28de-141">If you want to **rollover the cert**, then specify the new cert as primary and moving the current primary as secondary.</span></span> <span data-ttu-id="f28de-142">Výsledkem výměny aktuální primární certifikátu na nový certifikát v jednom kroku nasazení.</span><span class="sxs-lookup"><span data-stu-id="f28de-142">This results in the rollover of your current primary certificate to the new certificate in one deployment step.</span></span>

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


<span data-ttu-id="f28de-143">**Krok 4:** provádět změny **všechny** **Microsoft.Compute/virtualMachineScaleSets** definice prostředků - vyhledání Microsoft.Compute/virtualMachineScaleSets definici prostředků.</span><span class="sxs-lookup"><span data-stu-id="f28de-143">**Step 4:** Make changes to **all** the **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate the Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="f28de-144">Posuňte se "vydavatel": "Microsoft.Azure.ServiceFabric" v části "virtualMachineProfile".</span><span class="sxs-lookup"><span data-stu-id="f28de-144">Scroll to the "publisher": "Microsoft.Azure.ServiceFabric", under "virtualMachineProfile".</span></span>

<span data-ttu-id="f28de-145">V nastavení vydavatele služby infrastruktury by měl zobrazit něco podobného.</span><span class="sxs-lookup"><span data-stu-id="f28de-145">In the service fabric publisher settings, you should see something like this.</span></span>

![Json_Pub_Setting1][Json_Pub_Setting1]

<span data-ttu-id="f28de-147">Do ní přidejte nové položky certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f28de-147">Add the new cert entries to it</span></span>

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

<span data-ttu-id="f28de-148">Vlastnosti by teď měl vypadat takto</span><span class="sxs-lookup"><span data-stu-id="f28de-148">The properties should now look like this</span></span>

![Json_Pub_Setting2][Json_Pub_Setting2]

<span data-ttu-id="f28de-150">Pokud chcete **výměny certifikát**, zadejte nový certifikát jako primární a přesunutí aktuální primární jako sekundární.</span><span class="sxs-lookup"><span data-stu-id="f28de-150">If you want to **rollover the cert**, then specify the new cert as primary and moving the current primary as secondary.</span></span> <span data-ttu-id="f28de-151">Výsledkem výměny aktuální certifikát na nový certifikát v jednom kroku nasazení.</span><span class="sxs-lookup"><span data-stu-id="f28de-151">This results in the rollover of your current certificate to the new certificate in one deployment step.</span></span> 


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
<span data-ttu-id="f28de-152">Vlastnosti by teď měl vypadat takto</span><span class="sxs-lookup"><span data-stu-id="f28de-152">The properties should now look like this</span></span>

![Json_Pub_Setting3][Json_Pub_Setting3]


<span data-ttu-id="f28de-154">**Krok 5:** provádět změny **všechny** **Microsoft.Compute/virtualMachineScaleSets** definice prostředků - vyhledání Microsoft.Compute/virtualMachineScaleSets definici prostředků.</span><span class="sxs-lookup"><span data-stu-id="f28de-154">**Step 5:** Make Changes to **all** the **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate the Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="f28de-155">Posuňte se "vaultCertificates":, v části "OSProfile".</span><span class="sxs-lookup"><span data-stu-id="f28de-155">Scroll to the "vaultCertificates": , under "OSProfile".</span></span> <span data-ttu-id="f28de-156">by měla vypadat přibližně takto.</span><span class="sxs-lookup"><span data-stu-id="f28de-156">it should look something like this.</span></span>


![Json_Pub_Setting4][Json_Pub_Setting4]

<span data-ttu-id="f28de-158">SecCertificateUrlValue přidejte do ní.</span><span class="sxs-lookup"><span data-stu-id="f28de-158">Add the secCertificateUrlValue to it.</span></span> <span data-ttu-id="f28de-159">použijte následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f28de-159">use the following snippet:</span></span>

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
<span data-ttu-id="f28de-160">Výsledný formát Json by měl nyní vypadat přibližně takto.</span><span class="sxs-lookup"><span data-stu-id="f28de-160">Now the resulting Json should look something like this.</span></span>
<span data-ttu-id="f28de-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span><span class="sxs-lookup"><span data-stu-id="f28de-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span></span>


> [!NOTE]
> <span data-ttu-id="f28de-162">Ujistěte se, že obsahovat opakované kroky 4 a 5 pro všechny Nodetypes/Microsoft.Compute/virtualMachineScaleSets definice prostředků ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="f28de-162">Make sure that you have repeated steps 4 and 5 for all the Nodetypes/Microsoft.Compute/virtualMachineScaleSets resource definitions in your template.</span></span> <span data-ttu-id="f28de-163">Pokud jeden z nich přeskočíte, certifikát získat nenainstalují VMSS a že vést k neočekávaným výsledkům v clusteru, včetně clusteru směrem dolů (Pokud skončili žádné platné certifikáty, které může cluster používat pro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f28de-163">If you miss one of them, the certificate will not get installed on that VMSS and you will have unpredictable results in your cluster, including the cluster going down (if you end up with no valid certificates that the cluster can use for security.</span></span> <span data-ttu-id="f28de-164">Proto prosím Překontrolujte, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="f28de-164">So please double check, before proceeding further.</span></span>
> 
> 


### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a><span data-ttu-id="f28de-165">Upravte svůj soubor šablony tak, aby odrážely novou parametry, které jste přidali výše</span><span class="sxs-lookup"><span data-stu-id="f28de-165">Edit your template file to reflect the new parameters you added above</span></span>
<span data-ttu-id="f28de-166">Pokud používáte ukázku z [úložiště git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) se podle nich zorientujete, můžete začít proveďte změny v ukázkové 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON</span><span class="sxs-lookup"><span data-stu-id="f28de-166">If you are using the sample from the [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) to follow along, you can start to make changes in The sample 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON</span></span> 

<span data-ttu-id="f28de-167">Upravit daný parametr šablony Resource Manageru soubor, přidejte dva nové parametry pro secCertificateThumbprint a secCertificateUrlValue.</span><span class="sxs-lookup"><span data-stu-id="f28de-167">Edit your Resource Manager Template parameter File, add the two new parameters for secCertificateThumbprint and secCertificateUrlValue.</span></span> 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-the-template-to-azure"></a><span data-ttu-id="f28de-168">Nasazení šablony Azure</span><span class="sxs-lookup"><span data-stu-id="f28de-168">Deploy the template to Azure</span></span>

- <span data-ttu-id="f28de-169">Nyní jste připraveni k nasazení vaší šablony do Azure.</span><span class="sxs-lookup"><span data-stu-id="f28de-169">You are now ready to deploy your template to Azure.</span></span> <span data-ttu-id="f28de-170">Otevřete příkazový řádek se verze 1 + Azure PS.</span><span class="sxs-lookup"><span data-stu-id="f28de-170">Open an Azure PS version 1+ command prompt.</span></span>
- <span data-ttu-id="f28de-171">Přihlaste se k účtu Azure a vybrat konkrétní předplatné azure.</span><span class="sxs-lookup"><span data-stu-id="f28de-171">Log in to your Azure Account and select the specific azure subscription.</span></span> <span data-ttu-id="f28de-172">Toto je důležitý krok pro zaměstnance, kteří mají přístup k více než jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="f28de-172">This is an important step for folks who have access to more than one azure subscription.</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

<span data-ttu-id="f28de-173">Testování šablony před jeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="f28de-173">Test the template prior to deploying it.</span></span> <span data-ttu-id="f28de-174">Použijte stejnou skupinu prostředků clusteru aktuálně nasazené na.</span><span class="sxs-lookup"><span data-stu-id="f28de-174">Use the same Resource Group that your cluster is currently deployed to.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

<span data-ttu-id="f28de-175">Nasazení šablony do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="f28de-175">Deploy the template to your resource group.</span></span> <span data-ttu-id="f28de-176">Použijte stejnou skupinu prostředků clusteru aktuálně nasazené na.</span><span class="sxs-lookup"><span data-stu-id="f28de-176">Use the same Resource Group that your cluster is currently deployed to.</span></span> <span data-ttu-id="f28de-177">Spuštěním příkazu New-AzureRmResourceGroupDeployment.</span><span class="sxs-lookup"><span data-stu-id="f28de-177">Run the New-AzureRmResourceGroupDeployment command.</span></span> <span data-ttu-id="f28de-178">Není potřeba určit režimu, protože výchozí hodnota je **přírůstkové**.</span><span class="sxs-lookup"><span data-stu-id="f28de-178">You do not need to specify the mode, since the default value is **incremental**.</span></span>

> [!NOTE]
> <span data-ttu-id="f28de-179">Pokud nastavíte režim na dokončeno, můžete nechtěně odstranit prostředky, které nejsou ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="f28de-179">If you set Mode to Complete, you can inadvertently delete resources that are not in your template.</span></span> <span data-ttu-id="f28de-180">Nepoužívejte ho v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="f28de-180">So do not use it in this scenario.</span></span>
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

<span data-ttu-id="f28de-181">Tady je příklad vyplněné stejné prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f28de-181">Here is a filled out example of the same powershell.</span></span>

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

<span data-ttu-id="f28de-182">Po dokončení nasazení se připojit ke clusteru pomocí nového certifikátu a provádět některé dotazy.</span><span class="sxs-lookup"><span data-stu-id="f28de-182">Once the deployment is complete, connect to your cluster using the new Certificate and perform some queries.</span></span> <span data-ttu-id="f28de-183">Pokud budete moci provést.</span><span class="sxs-lookup"><span data-stu-id="f28de-183">If you are able to do.</span></span> <span data-ttu-id="f28de-184">Potom můžete odstranit starý certifikát.</span><span class="sxs-lookup"><span data-stu-id="f28de-184">Then you can delete the old certificate.</span></span> 

<span data-ttu-id="f28de-185">Pokud používáte certifikát podepsaný svým držitelem, nezapomeňte znovu importujte je do místního úložiště certifikátů TrustedPeople.</span><span class="sxs-lookup"><span data-stu-id="f28de-185">If you are using a self-signed certificate, do not forget to import them into your local TrustedPeople cert store.</span></span>

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
<span data-ttu-id="f28de-186">Pro rychlou referenci tady je příkaz k připojení do clusteru s podporou zabezpečení</span><span class="sxs-lookup"><span data-stu-id="f28de-186">For quick reference here is the command to connect to a secure cluster</span></span> 

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
<span data-ttu-id="f28de-187">Pro rychlou referenci tady je příkaz pro získání stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="f28de-187">For quick reference here is the command to get cluster health</span></span>

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-to-the-cluster"></a><span data-ttu-id="f28de-188">Nasazení certifikátů aplikací do clusteru.</span><span class="sxs-lookup"><span data-stu-id="f28de-188">Deploying Application certificates to the cluster.</span></span>

<span data-ttu-id="f28de-189">Jak je uvedeno v kroky 5 výše můžete použít stejný postup certifikátů z keyvault nasazena do uzlů.</span><span class="sxs-lookup"><span data-stu-id="f28de-189">You can use the same steps as outlined in Steps 5 above to have the certificates deployed from a keyvault to the Nodes.</span></span> <span data-ttu-id="f28de-190">Stačí nutné definovat a použít jiné parametry.</span><span class="sxs-lookup"><span data-stu-id="f28de-190">you just need define and use different parameters.</span></span>


## <a name="adding-or-removing-client-certificates"></a><span data-ttu-id="f28de-191">Přidání nebo odebrání klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="f28de-191">Adding or removing Client certificates</span></span>

<span data-ttu-id="f28de-192">Kromě certifikátů clusteru můžete přidat klientských certifikátů mohli provádět operace správy na service fabric cluster.</span><span class="sxs-lookup"><span data-stu-id="f28de-192">In addition to the cluster certificates, you can add client certificates to perform management operations on a service fabric cluster.</span></span>

<span data-ttu-id="f28de-193">Můžete přidat dva druhy klientské certifikáty - správce nebo jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="f28de-193">You can add two kinds of client certificates - Admin or Read-only.</span></span> <span data-ttu-id="f28de-194">Tyto pak můžete použít k řízení přístupu k dotazu operace na clusteru a operace správce.</span><span class="sxs-lookup"><span data-stu-id="f28de-194">These then can be used to control access to the admin operations and Query operations on the cluster.</span></span> <span data-ttu-id="f28de-195">Ve výchozím nastavení jsou certifikáty clusteru přidat do seznamu povolených certifikáty správce.</span><span class="sxs-lookup"><span data-stu-id="f28de-195">By default, the cluster certificates are added to the allowed Admin certificates list.</span></span>

<span data-ttu-id="f28de-196">můžete zadat libovolný počet klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="f28de-196">you can specify any number of client certificates.</span></span> <span data-ttu-id="f28de-197">Každý přidávání a odstraňování výsledků v aktualizaci konfigurace service fabric cluster</span><span class="sxs-lookup"><span data-stu-id="f28de-197">Each addition/deletion results in a configuration update to the service fabric cluster</span></span>


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a><span data-ttu-id="f28de-198">Přidání klientské certifikáty - správce nebo jen pro čtení přes portál</span><span class="sxs-lookup"><span data-stu-id="f28de-198">Adding client certificates - Admin or Read-Only via portal</span></span>

1. <span data-ttu-id="f28de-199">Přejděte do okna zabezpečení a vyberte '+ ověřování' tlačítka v okně zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f28de-199">Navigate to the Security blade, and select the '+ Authentication' button on top of the security blade.</span></span>
2. <span data-ttu-id="f28de-200">V okně "přidat ověření vyberte"Ověřování typu"- 'jen pro čtení klienta' nebo 'správce klienta.</span><span class="sxs-lookup"><span data-stu-id="f28de-200">On the 'Add Authentication' blade, choose the 'Authentication Type' - 'Read-only client' or 'Admin client'</span></span>
3. <span data-ttu-id="f28de-201">Teď zvolte metoda autorizace.</span><span class="sxs-lookup"><span data-stu-id="f28de-201">Now choose the Authorization method.</span></span> <span data-ttu-id="f28de-202">To znamená do Service Fabric, zda by měla vypadat tohoto certifikátu pomocí názvu subjektu nebo kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="f28de-202">This indicates to Service Fabric whether it should look up this certificate by using the subject name or the thumbprint.</span></span> <span data-ttu-id="f28de-203">Obecně platí není dobrým zvykem lze pomocí této metody ověřování názvu subjektu.</span><span class="sxs-lookup"><span data-stu-id="f28de-203">In general, it is not a good security practice to use the authorization method of subject name.</span></span> 

![Přidání certifikátu klienta][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-the-portal"></a><span data-ttu-id="f28de-205">Odstranění klientské certifikáty - správce nebo jen pro čtení pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="f28de-205">Deletion of Client Certificates - Admin or Read-Only using the portal</span></span>

<span data-ttu-id="f28de-206">K odebrání sekundární certifikátu používá pro zabezpečení clusteru, přejděte do okna zabezpečení a vyberte možnost 'Delete' z kontextové nabídky na konkrétní certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f28de-206">To remove a secondary certificate from being used for cluster security, Navigate to the Security blade and select the 'Delete' option from the context menu on the specific certificate.</span></span>



## <a name="next-steps"></a><span data-ttu-id="f28de-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f28de-207">Next steps</span></span>
<span data-ttu-id="f28de-208">Přečtěte si tyto články pro další informace o správě clusteru:</span><span class="sxs-lookup"><span data-stu-id="f28de-208">Read these articles for more information on cluster management:</span></span>

* [<span data-ttu-id="f28de-209">Proces upgradu Service Fabric Cluster a očekávání od vás</span><span class="sxs-lookup"><span data-stu-id="f28de-209">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="f28de-210">Instalační program přístup na základě rolí pro klienty</span><span class="sxs-lookup"><span data-stu-id="f28de-210">Setup role-based access for clients</span></span>](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


