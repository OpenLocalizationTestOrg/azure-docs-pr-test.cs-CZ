---
title: "Vytvoření identity pro aplikaci Azure pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Popisuje, jak používat rozhraní příkazového řádku Azure k vytvoření aplikace Azure Active Directory a objektu zabezpečení a jí udělit přístup k prostředkům prostřednictvím řízení přístupu na základě rolí. Ukazuje, jak k ověření aplikace pomocí hesla nebo certifikátu."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 3c5826d58887ff1af4df8e66999d9c1a1643bcc7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a><span data-ttu-id="de5d1-104">Vytvoření instančního objektu pro přístup k prostředkům pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="de5d1-104">Use Azure CLI to create a service principal to access resources</span></span>

<span data-ttu-id="de5d1-105">Pokud máte aplikace nebo skriptu, která potřebuje přístup k prostředkům, můžete nastavit identity aplikace a ověřit aplikaci s svoje vlastní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="de5d1-105">When you have an app or script that needs to access resources, you can set up an identity for the app and authenticate the app with its own credentials.</span></span> <span data-ttu-id="de5d1-106">Tato identita se označuje jako hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="de5d1-106">This identity is known as a service principal.</span></span> <span data-ttu-id="de5d1-107">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="de5d1-107">This approach enables you to:</span></span>

* <span data-ttu-id="de5d1-108">Přiřadíte oprávnění k identitě aplikace, která se liší od vlastní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="de5d1-108">Assign permissions to the app identity that are different than your own permissions.</span></span> <span data-ttu-id="de5d1-109">Tato oprávnění jsou obvykle omezené na přesně co aplikaci je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="de5d1-109">Typically, these permissions are restricted to exactly what the app needs to do.</span></span>
* <span data-ttu-id="de5d1-110">Při provádění bezobslužného skriptu, použijte certifikát pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="de5d1-110">Use a certificate for authentication when executing an unattended script.</span></span>

<span data-ttu-id="de5d1-111">V tomto článku se dozvíte, jak používat [Azure CLI 1.0](../cli-install-nodejs.md) nastavit aplikaci pro spuštění pod svou vlastní pověření a identity.</span><span class="sxs-lookup"><span data-stu-id="de5d1-111">This article shows you how to use [Azure CLI 1.0](../cli-install-nodejs.md) to set up an application to run under its own credentials and identity.</span></span> <span data-ttu-id="de5d1-112">Nainstalujte nejnovější verzi [Azure CLI 1.0](../cli-install-nodejs.md) prostředí odpovídá v příkladech v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="de5d1-112">Install the latest version of [Azure CLI 1.0](../cli-install-nodejs.md) to make sure your environment matches the examples in this article.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="de5d1-113">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="de5d1-113">Required permissions</span></span>
<span data-ttu-id="de5d1-114">K dokončení tohoto tématu, musíte mít dostatečná oprávnění v Azure Active Directory a vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="de5d1-114">To complete this topic, you must have sufficient permissions in both your Azure Active Directory and your Azure subscription.</span></span> <span data-ttu-id="de5d1-115">Konkrétně musí být schopná vytvořit aplikaci ve službě Azure Active Directory a přiřazení objektu služby roli.</span><span class="sxs-lookup"><span data-stu-id="de5d1-115">Specifically, you must be able to create an app in the Azure Active Directory, and assign the service principal to a role.</span></span> 

<span data-ttu-id="de5d1-116">Nejjednodušším způsobem, jak zkontrolovat, jestli má váš účet dostatečná oprávnění, je použít k tomu portál.</span><span class="sxs-lookup"><span data-stu-id="de5d1-116">The easiest way to check whether your account has adequate permissions is through the portal.</span></span> <span data-ttu-id="de5d1-117">Informace najdete v článku [Kontrola požadovaných oprávnění na portálu](resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="de5d1-117">See [Check required permission in portal](resource-group-create-service-principal-portal.md#required-permissions).</span></span>

<span data-ttu-id="de5d1-118">Nyní, přejít na část pro buď [heslo](#create-service-principal-with-password) nebo [certifikát](#create-service-principal-with-certificate) ověřování.</span><span class="sxs-lookup"><span data-stu-id="de5d1-118">Now, proceed to a section for either [password](#create-service-principal-with-password) or [certificate](#create-service-principal-with-certificate) authentication.</span></span>

## <a name="create-service-principal-with-password"></a><span data-ttu-id="de5d1-119">Vytvoření instančního objektu s heslem</span><span class="sxs-lookup"><span data-stu-id="de5d1-119">Create service principal with password</span></span>
<span data-ttu-id="de5d1-120">V této části můžete provést kroky k vytvoření aplikace AD s heslem a přiřadit role Čtenář instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="de5d1-120">In this section, you perform the steps to create the AD application with a password, and assign the Reader role to the service principal.</span></span>

1. <span data-ttu-id="de5d1-121">Přihlaste se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="de5d1-121">Sign in to your account.</span></span>
   
   ```azurecli
   azure login
   ```
2. <span data-ttu-id="de5d1-122">K vytvoření identity aplikace, zadejte název aplikace a hesla, jak je znázorněno v následujícím příkazu:</span><span class="sxs-lookup"><span data-stu-id="de5d1-122">To create an app identity, provide the name of the app and a password, as shown in the following command:</span></span>
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   <span data-ttu-id="de5d1-123">Vrátí se nový instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="de5d1-123">The new service principal is returned.</span></span> <span data-ttu-id="de5d1-124">Id objektu je potřeba při udělování oprávnění.</span><span class="sxs-lookup"><span data-stu-id="de5d1-124">The Object Id is needed when granting permissions.</span></span> <span data-ttu-id="de5d1-125">Identifikátor guid uvedený s textem hlavní názvy služby je potřeba při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="de5d1-125">The guid listed with the Service Principal Names is needed when logging in.</span></span> <span data-ttu-id="de5d1-126">Tento identifikátor guid je stejnou hodnotu jako id aplikace.</span><span class="sxs-lookup"><span data-stu-id="de5d1-126">This guid is the same value as the app id.</span></span> <span data-ttu-id="de5d1-127">V ukázkových aplikací, tato hodnota se označuje jako `Client ID`.</span><span class="sxs-lookup"><span data-stu-id="de5d1-127">In the sample applications, this value is referred to as the `Client ID`.</span></span> 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. <span data-ttu-id="de5d1-128">Udělte oprávnění objektu služby pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="de5d1-128">Grant the service principal permissions on your subscription.</span></span> <span data-ttu-id="de5d1-129">V tomto příkladu přidáte objektu služby roli čtečka, která uděluje oprávnění ke čtení všechny prostředky v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="de5d1-129">In this example, you add the service principal to the Reader role, which grants permission to read all resources in the subscription.</span></span> <span data-ttu-id="de5d1-130">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="de5d1-130">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span> <span data-ttu-id="de5d1-131">Pro parametr objectid zadejte Id objektu, který jste použili při vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="de5d1-131">For the objectid parameter, provide the Object Id that you used when creating the application.</span></span> <span data-ttu-id="de5d1-132">Před spuštěním tohoto příkazu, musíte povolit nějakou dobu nového objektu služby, aby se rozšířily změny v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="de5d1-132">Before running this command, you must allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="de5d1-133">Když ručně spustíte tyto příkazy, obvykle dostatek času uplynul mezi úkoly.</span><span class="sxs-lookup"><span data-stu-id="de5d1-133">When you run these commands manually, usually enough time has elapsed between tasks.</span></span> <span data-ttu-id="de5d1-134">Ve skriptu, měli byste přidat krok do režimu spánku mezi příkazy (například `sleep 15`).</span><span class="sxs-lookup"><span data-stu-id="de5d1-134">In a script, you should add a step to sleep between the commands (like `sleep 15`).</span></span> <span data-ttu-id="de5d1-135">Pokud se zobrazí chyba oznamující, že objekt zabezpečení neexistuje v adresáři, spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="de5d1-135">If you see an error stating the principal does not exist in the directory, rerun the command.</span></span>
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
<span data-ttu-id="de5d1-136">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="de5d1-136">That's it!</span></span> <span data-ttu-id="de5d1-137">Aplikaci AD a instanční objekt se nastavují.</span><span class="sxs-lookup"><span data-stu-id="de5d1-137">Your AD application and service principal are set up.</span></span> <span data-ttu-id="de5d1-138">V další části se dozvíte, jak k přihlášení pomocí přihlašovacích údajů prostřednictvím rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="de5d1-138">The next section shows you how to log in with the credential through Azure CLI.</span></span> <span data-ttu-id="de5d1-139">Pokud chcete použít přihlašovací údaje v kódu aplikace, není potřeba před pokračováním v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="de5d1-139">If you want to use the credential in your code application, you do not need to continue with this topic.</span></span> <span data-ttu-id="de5d1-140">Můžete přejít na [ukázkové aplikace](#sample-applications) příklady přihlášení pomocí id aplikace a heslo.</span><span class="sxs-lookup"><span data-stu-id="de5d1-140">You can jump to the [Sample applications](#sample-applications) for examples of logging in with your application id and password.</span></span> 

### <a name="provide-credentials-through-azure-cli"></a><span data-ttu-id="de5d1-141">Zadejte přihlašovací údaje prostřednictvím rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="de5d1-141">Provide credentials through Azure CLI</span></span>
<span data-ttu-id="de5d1-142">Nyní budete muset přihlásit jako aplikace k provádění operací.</span><span class="sxs-lookup"><span data-stu-id="de5d1-142">Now, you need to log in as the application to perform operations.</span></span>

1. <span data-ttu-id="de5d1-143">Vždy, když se přihlásíte jako hlavní název služby, je třeba zadat id klienta adresáře pro vaši aplikaci AD.</span><span class="sxs-lookup"><span data-stu-id="de5d1-143">Whenever you sign in as a service principal, you need to provide the tenant id of the directory for your AD app.</span></span> <span data-ttu-id="de5d1-144">Klient je instance služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="de5d1-144">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="de5d1-145">Pro načtení id klienta pro vaše předplatné aktuálně ověřený, použijte:</span><span class="sxs-lookup"><span data-stu-id="de5d1-145">To retrieve the tenant id for your currently authenticated subscription, use:</span></span>
   
   ```azurecli
   azure account show
   ```
   
   <span data-ttu-id="de5d1-146">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="de5d1-146">Which returns:</span></span>
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     <span data-ttu-id="de5d1-147">Pokud potřebujete získat id klienta jiné předplatné, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="de5d1-147">If you need to get the tenant id of another subscription, use the following command:</span></span>
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. <span data-ttu-id="de5d1-148">Pokud budete potřebovat načíst id klienta, který chcete použít pro přihlášení, použijte:</span><span class="sxs-lookup"><span data-stu-id="de5d1-148">If you need to retrieve the client id to use for logging in, use:</span></span>
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     <span data-ttu-id="de5d1-149">Hodnota, která má použít pro přihlášení je identifikátor guid uvedený v hlavní názvy služby.</span><span class="sxs-lookup"><span data-stu-id="de5d1-149">The value to use for logging in is the guid listed in the service principal names.</span></span>
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. <span data-ttu-id="de5d1-150">Přihlaste se jako objekt služby.</span><span class="sxs-lookup"><span data-stu-id="de5d1-150">Log in as the service principal.</span></span>
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    <span data-ttu-id="de5d1-151">Zobrazí se výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="de5d1-151">You are prompted for the password.</span></span> <span data-ttu-id="de5d1-152">Zadejte heslo, které jste zadali při vytváření aplikace AD.</span><span class="sxs-lookup"><span data-stu-id="de5d1-152">Provide the password you specified when creating the AD application.</span></span>
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

<span data-ttu-id="de5d1-153">Nyní jste přihlášeni jako objekt služby pro objekt služby, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="de5d1-153">You are now authenticated as the service principal for the service principal that you created.</span></span>

<span data-ttu-id="de5d1-154">Alternativně můžete vyvolat operace REST z příkazového řádku k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="de5d1-154">Alternatively, you can invoke REST operations from the command line to log in.</span></span> <span data-ttu-id="de5d1-155">Z odpovědi ověřování může získat přístupový token pro použití s dalšími operacemi.</span><span class="sxs-lookup"><span data-stu-id="de5d1-155">From the authentication response, you can retrieve the access token for use with other operations.</span></span> <span data-ttu-id="de5d1-156">Příklad získání tokenu přístupu vyvoláním operace REST, naleznete v části [generování tokenu přístupu](resource-manager-rest-api.md#generating-an-access-token).</span><span class="sxs-lookup"><span data-stu-id="de5d1-156">For an example of retrieving the access token by invoking REST operations, see [Generating an Access Token](resource-manager-rest-api.md#generating-an-access-token).</span></span>

## <a name="create-service-principal-with-certificate"></a><span data-ttu-id="de5d1-157">Vytvoření instančního objektu s certifikátem</span><span class="sxs-lookup"><span data-stu-id="de5d1-157">Create service principal with certificate</span></span>
<span data-ttu-id="de5d1-158">V této části proveďte postup:</span><span class="sxs-lookup"><span data-stu-id="de5d1-158">In this section, you perform the steps to:</span></span>

* <span data-ttu-id="de5d1-159">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="de5d1-159">create a self-signed certificate</span></span>
* <span data-ttu-id="de5d1-160">Vytvořte aplikaci AD se certifikát a instanční objekt</span><span class="sxs-lookup"><span data-stu-id="de5d1-160">create the AD application with the certificate, and the service principal</span></span>
* <span data-ttu-id="de5d1-161">přiřadit role Čtenář instančního objektu</span><span class="sxs-lookup"><span data-stu-id="de5d1-161">assign the Reader role to the service principal</span></span>

<span data-ttu-id="de5d1-162">K provedení těchto kroků, musíte mít [OpenSSL](http://www.openssl.org/) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="de5d1-162">To complete these steps, you must have [OpenSSL](http://www.openssl.org/) installed.</span></span>

1. <span data-ttu-id="de5d1-163">Vytvořte certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="de5d1-163">Create a self-signed certificate.</span></span>
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. <span data-ttu-id="de5d1-164">V předchozím kroku vytvořili dva soubory - privkey.pem a cert.pem.</span><span class="sxs-lookup"><span data-stu-id="de5d1-164">The preceding step created two files - privkey.pem and cert.pem.</span></span> <span data-ttu-id="de5d1-165">Veřejné a soukromé klíče zkombinovat do jednoho souboru.</span><span class="sxs-lookup"><span data-stu-id="de5d1-165">Combine the public and private keys into a single file.</span></span>

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. <span data-ttu-id="de5d1-166">Otevřete **examplecert.pem** souborů a vyhledejte dlouhé posloupnosti znaků mezi **---BEGIN CERTIFICATE---** a **---END CERTIFICATE---**.</span><span class="sxs-lookup"><span data-stu-id="de5d1-166">Open the **examplecert.pem** file and look for the long sequence of characters between **-----BEGIN CERTIFICATE-----** and **-----END CERTIFICATE-----**.</span></span> <span data-ttu-id="de5d1-167">Zkopírujte data certifikátu.</span><span class="sxs-lookup"><span data-stu-id="de5d1-167">Copy the certificate data.</span></span> <span data-ttu-id="de5d1-168">Při vytváření služby hlavní předáte tato data jako parametr.</span><span class="sxs-lookup"><span data-stu-id="de5d1-168">You pass this data as a parameter when creating the service principal.</span></span>

4. <span data-ttu-id="de5d1-169">Přihlaste se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="de5d1-169">Sign in to your account.</span></span>

   ```azurecli
   azure login
   ```
5. <span data-ttu-id="de5d1-170">Pokud chcete vytvořit objekt služby, zadejte název aplikace a data certifikátu, jak je znázorněno v následujícím příkazu:</span><span class="sxs-lookup"><span data-stu-id="de5d1-170">To create the service principal, provide the name of the app and the certificate data, as shown in the following command:</span></span>
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   <span data-ttu-id="de5d1-171">Vrátí se nový instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="de5d1-171">The new service principal is returned.</span></span> <span data-ttu-id="de5d1-172">Id objektu je potřeba při udělování oprávnění.</span><span class="sxs-lookup"><span data-stu-id="de5d1-172">The Object Id is needed when granting permissions.</span></span> <span data-ttu-id="de5d1-173">Identifikátor guid uvedený s textem hlavní názvy služby je potřeba při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="de5d1-173">The guid listed with the Service Principal Names is needed when logging in.</span></span> <span data-ttu-id="de5d1-174">Tento identifikátor guid je stejnou hodnotu jako id aplikace.</span><span class="sxs-lookup"><span data-stu-id="de5d1-174">This guid is the same value as the app id.</span></span> <span data-ttu-id="de5d1-175">V ukázkových aplikací tato hodnota se označuje jako ID klienta.</span><span class="sxs-lookup"><span data-stu-id="de5d1-175">In the sample applications, this value is referred to as the Client ID.</span></span> 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. <span data-ttu-id="de5d1-176">Udělte oprávnění objektu služby pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="de5d1-176">Grant the service principal permissions on your subscription.</span></span> <span data-ttu-id="de5d1-177">V tomto příkladu přidáte objektu služby roli čtečka, která uděluje oprávnění ke čtení všechny prostředky v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="de5d1-177">In this example, you add the service principal to the Reader role, which grants permission to read all resources in the subscription.</span></span> <span data-ttu-id="de5d1-178">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="de5d1-178">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span> <span data-ttu-id="de5d1-179">Pro parametr objectid zadejte Id objektu, který jste použili při vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="de5d1-179">For the objectid parameter, provide the Object Id that you used when creating the application.</span></span> <span data-ttu-id="de5d1-180">Před spuštěním tohoto příkazu, musíte povolit nějakou dobu nového objektu služby, aby se rozšířily změny v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="de5d1-180">Before running this command, you must allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="de5d1-181">Když ručně spustíte tyto příkazy, obvykle dostatek času uplynul mezi úkoly.</span><span class="sxs-lookup"><span data-stu-id="de5d1-181">When you run these commands manually, usually enough time has elapsed between tasks.</span></span> <span data-ttu-id="de5d1-182">Ve skriptu, měli byste přidat krok do režimu spánku mezi příkazy (například `sleep 15`).</span><span class="sxs-lookup"><span data-stu-id="de5d1-182">In a script, you should add a step to sleep between the commands (like `sleep 15`).</span></span> <span data-ttu-id="de5d1-183">Pokud se zobrazí chyba oznamující, že objekt zabezpečení neexistuje v adresáři, spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="de5d1-183">If you see an error stating the principal does not exist in the directory, rerun the command.</span></span>
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a><span data-ttu-id="de5d1-184">Zadejte certifikát pomocí automatizované skriptu rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="de5d1-184">Provide certificate through automated Azure CLI script</span></span>
<span data-ttu-id="de5d1-185">Nyní budete muset přihlásit jako aplikace k provádění operací.</span><span class="sxs-lookup"><span data-stu-id="de5d1-185">Now, you need to log in as the application to perform operations.</span></span>

1. <span data-ttu-id="de5d1-186">Vždy, když se přihlásíte jako hlavní název služby, je třeba zadat id klienta adresáře pro vaši aplikaci AD.</span><span class="sxs-lookup"><span data-stu-id="de5d1-186">Whenever you sign in as a service principal, you need to provide the tenant id of the directory for your AD app.</span></span> <span data-ttu-id="de5d1-187">Klient je instance služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="de5d1-187">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="de5d1-188">Pro načtení id klienta pro vaše předplatné aktuálně ověřený, použijte:</span><span class="sxs-lookup"><span data-stu-id="de5d1-188">To retrieve the tenant id for your currently authenticated subscription, use:</span></span>
   
   ```azurecli
   azure account show
   ```
   
   <span data-ttu-id="de5d1-189">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="de5d1-189">Which returns:</span></span>
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   <span data-ttu-id="de5d1-190">Pokud potřebujete získat id klienta jiné předplatné, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="de5d1-190">If you need to get the tenant id of another subscription, use the following command:</span></span>
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. <span data-ttu-id="de5d1-191">Pokud chcete načíst kryptografický otisk certifikátu a odeberte nepotřebné znaky, použijte:</span><span class="sxs-lookup"><span data-stu-id="de5d1-191">To retrieve the certificate thumbprint and remove unneeded characters, use:</span></span>
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   <span data-ttu-id="de5d1-192">Která vrací podobně jako hodnota kryptografického otisku:</span><span class="sxs-lookup"><span data-stu-id="de5d1-192">Which returns a thumbprint value similar to:</span></span>
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. <span data-ttu-id="de5d1-193">Pokud budete potřebovat načíst id klienta, který chcete použít pro přihlášení, použijte:</span><span class="sxs-lookup"><span data-stu-id="de5d1-193">If you need to retrieve the client id to use for logging in, use:</span></span>
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   <span data-ttu-id="de5d1-194">Hodnota, která má použít pro přihlášení je identifikátor guid uvedený v hlavní názvy služby.</span><span class="sxs-lookup"><span data-stu-id="de5d1-194">The value to use for logging in is the guid listed in the service principal names.</span></span>
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. <span data-ttu-id="de5d1-195">Přihlaste se jako objekt služby.</span><span class="sxs-lookup"><span data-stu-id="de5d1-195">Log in as the service principal.</span></span>
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

<span data-ttu-id="de5d1-196">Nyní jste přihlášeni jako hlavní služby pro aplikaci Azure Active Directory, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="de5d1-196">You are now authenticated as the service principal for the Azure Active Directory application that you created.</span></span>

## <a name="change-credentials"></a><span data-ttu-id="de5d1-197">Změnit pověření</span><span class="sxs-lookup"><span data-stu-id="de5d1-197">Change credentials</span></span>

<span data-ttu-id="de5d1-198">Chcete-li změnit přihlašovací údaje pro aplikaci AD, buď kvůli ohrožení zabezpečení nebo vypršení platnosti pověření použít `azure ad app set`.</span><span class="sxs-lookup"><span data-stu-id="de5d1-198">To change the credentials for an AD app, either because of a security compromise or a credential expiration, use `azure ad app set`.</span></span>

<span data-ttu-id="de5d1-199">Chcete-li změnit heslo, použijte:</span><span class="sxs-lookup"><span data-stu-id="de5d1-199">To change a password, use:</span></span>

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

<span data-ttu-id="de5d1-200">Chcete-li změnit hodnotu certifikát, použijte:</span><span class="sxs-lookup"><span data-stu-id="de5d1-200">To change a certificate value, use:</span></span>

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a><span data-ttu-id="de5d1-201">Ladění</span><span class="sxs-lookup"><span data-stu-id="de5d1-201">Debug</span></span>

<span data-ttu-id="de5d1-202">Při vytváření objektu služby se můžete setkat s následujícími chybami:</span><span class="sxs-lookup"><span data-stu-id="de5d1-202">You may encounter the following errors when creating a service principal:</span></span>

* <span data-ttu-id="de5d1-203">**"Authentication_Unauthorized"** nebo **"žádné předplatné nalezena v kontextu."**</span><span class="sxs-lookup"><span data-stu-id="de5d1-203">**"Authentication_Unauthorized"** or **"No subscription found in the context."**</span></span> <span data-ttu-id="de5d1-204">-Se zobrazí tato chyba, pokud váš účet nemá [požadovaná oprávnění](#required-permissions) v Azure Active Directory pro registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="de5d1-204">- You see this error when your account does not have the [required permissions](#required-permissions) on the Azure Active Directory to register an app.</span></span> <span data-ttu-id="de5d1-205">Obvykle se zobrazí tato chyba při jenom správci ve vašem Azure Active Directory můžete zaregistrovat aplikace a váš účet není správce.</span><span class="sxs-lookup"><span data-stu-id="de5d1-205">Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin.</span></span> <span data-ttu-id="de5d1-206">Požádejte správce buď můžete přiřadit role správce nebo umožňuje uživatelům registrovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="de5d1-206">Ask your administrator to either assign you to an administrator role, or to enable users to register apps.</span></span>

* <span data-ttu-id="de5d1-207">Váš účet **"nemá oprávnění k provedení akce 'Microsoft.Authorization/roleAssignments/write' u rozsahu: /subscriptions/ {guid}'."**  -Zobrazí tato chyba, pokud se váš účet nemá dostatečná oprávnění k přiřazení role identity.</span><span class="sxs-lookup"><span data-stu-id="de5d1-207">Your account **"does not have authorization to perform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions to assign a role to an identity.</span></span> <span data-ttu-id="de5d1-208">Požádejte správce předplatného můžete přidat do role správce přístupu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="de5d1-208">Ask your subscription administrator to add you to User Access Administrator role.</span></span>

## <a name="sample-applications"></a><span data-ttu-id="de5d1-209">Ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="de5d1-209">Sample applications</span></span>
<span data-ttu-id="de5d1-210">Informace o protokolování jako aplikace prostřednictvím různých platforem najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="de5d1-210">For information about logging in as the application through different platforms, see:</span></span>

* [<span data-ttu-id="de5d1-211">.NET</span><span class="sxs-lookup"><span data-stu-id="de5d1-211">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="de5d1-212">Java</span><span class="sxs-lookup"><span data-stu-id="de5d1-212">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="de5d1-213">Node.js</span><span class="sxs-lookup"><span data-stu-id="de5d1-213">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="de5d1-214">Python</span><span class="sxs-lookup"><span data-stu-id="de5d1-214">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="de5d1-215">Ruby</span><span class="sxs-lookup"><span data-stu-id="de5d1-215">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a><span data-ttu-id="de5d1-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de5d1-216">Next steps</span></span>
* <span data-ttu-id="de5d1-217">Podrobné pokyny k integraci aplikace do Azure pro správu prostředků najdete v tématu [Příručka pro vývojáře k autorizaci s rozhraním API pro Azure Resource Manager](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="de5d1-217">For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide to authorization with the Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="de5d1-218">Chcete-li získat další informace o používání certifikátů a rozhraní příkazového řádku Azure, najdete v části [ověřování prostřednictvím certifikátu s objekty služby Azure z příkazového řádku Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx).</span><span class="sxs-lookup"><span data-stu-id="de5d1-218">To get more information about using certificates and Azure CLI, see [Certificate-based authentication with Azure Service Principals from Linux command line](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx).</span></span> 
* <span data-ttu-id="de5d1-219">Seznam dostupných akcí, které může být povolen nebo odepřen uživatelům najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="de5d1-219">For a list of available actions that can be granted or denied to users, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>
