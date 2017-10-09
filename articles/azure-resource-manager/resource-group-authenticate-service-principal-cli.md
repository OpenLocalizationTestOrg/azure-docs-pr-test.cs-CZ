---
title: "aaaCreate identitu pro aplikace Azure pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Popisuje, jak řídit toouse rozhraní příkazového řádku Azure toocreate aplikaci Azure Active Directory a objektu služby a udělte ho tooresources přístup prostřednictvím přístupu na základě rolí. Ukazuje, jak tooauthenticate aplikace pomocí hesla nebo certifikátu."
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
ms.openlocfilehash: 0d693ec801d4f4d6c24ec420580776e73014b325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-cli-toocreate-a-service-principal-tooaccess-resources"></a><span data-ttu-id="64a63-104">Pomocí rozhraní příkazového řádku Azure toocreate hlavní tooaccess prostředky služby</span><span class="sxs-lookup"><span data-stu-id="64a63-104">Use Azure CLI toocreate a service principal tooaccess resources</span></span>

<span data-ttu-id="64a63-105">Pokud máte aplikace nebo skript, který potřebuje tooaccess prostředků, můžete nastavit identity aplikace hello a ověřit hello aplikaci s svoje vlastní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="64a63-105">When you have an app or script that needs tooaccess resources, you can set up an identity for hello app and authenticate hello app with its own credentials.</span></span> <span data-ttu-id="64a63-106">Tato identita se označuje jako hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="64a63-106">This identity is known as a service principal.</span></span> <span data-ttu-id="64a63-107">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="64a63-107">This approach enables you to:</span></span>

* <span data-ttu-id="64a63-108">Přiřazení oprávnění toohello identita aplikace, která se liší od vlastní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64a63-108">Assign permissions toohello app identity that are different than your own permissions.</span></span> <span data-ttu-id="64a63-109">Obvykle se tato oprávnění se s omezeným přístupem tooexactly jaké aplikace hello musí toodo.</span><span class="sxs-lookup"><span data-stu-id="64a63-109">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span>
* <span data-ttu-id="64a63-110">Při provádění bezobslužného skriptu, použijte certifikát pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="64a63-110">Use a certificate for authentication when executing an unattended script.</span></span>

<span data-ttu-id="64a63-111">Tento článek ukazuje, jak toouse [Azure CLI 1.0](../cli-install-nodejs.md) tooset až toorun aplikaci v části vlastní pověření a identity.</span><span class="sxs-lookup"><span data-stu-id="64a63-111">This article shows you how toouse [Azure CLI 1.0](../cli-install-nodejs.md) tooset up an application toorun under its own credentials and identity.</span></span> <span data-ttu-id="64a63-112">Nainstalujte nejnovější verzi k hello [Azure CLI 1.0](../cli-install-nodejs.md) toomake prostředí, která odpovídá hello příklady v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="64a63-112">Install hello latest version of [Azure CLI 1.0](../cli-install-nodejs.md) toomake sure your environment matches hello examples in this article.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="64a63-113">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="64a63-113">Required permissions</span></span>
<span data-ttu-id="64a63-114">toocomplete tohoto tématu, musíte mít dostatečná oprávnění v Azure Active Directory a vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="64a63-114">toocomplete this topic, you must have sufficient permissions in both your Azure Active Directory and your Azure subscription.</span></span> <span data-ttu-id="64a63-115">Konkrétně musí být schopný toocreate aplikace v hello Azure Active Directory a přiřadit role hlavní tooa služby hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-115">Specifically, you must be able toocreate an app in hello Azure Active Directory, and assign hello service principal tooa role.</span></span> 

<span data-ttu-id="64a63-116">Hello bylo jestli váš účet má odpovídající oprávnění je prostřednictvím portálu hello nejjednodušší toocheck způsobem.</span><span class="sxs-lookup"><span data-stu-id="64a63-116">hello easiest way toocheck whether your account has adequate permissions is through hello portal.</span></span> <span data-ttu-id="64a63-117">Informace najdete v článku [Kontrola požadovaných oprávnění na portálu](resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="64a63-117">See [Check required permission in portal](resource-group-create-service-principal-portal.md#required-permissions).</span></span>

<span data-ttu-id="64a63-118">Nyní, pokračovat tooa části pro buď [heslo](#create-service-principal-with-password) nebo [certifikát](#create-service-principal-with-certificate) ověřování.</span><span class="sxs-lookup"><span data-stu-id="64a63-118">Now, proceed tooa section for either [password](#create-service-principal-with-password) or [certificate](#create-service-principal-with-certificate) authentication.</span></span>

## <a name="create-service-principal-with-password"></a><span data-ttu-id="64a63-119">Vytvoření instančního objektu s heslem</span><span class="sxs-lookup"><span data-stu-id="64a63-119">Create service principal with password</span></span>
<span data-ttu-id="64a63-120">V této části provést hello kroky toocreate hello aplikaci AD s heslem a přiřaďte hello čtečky role toohello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="64a63-120">In this section, you perform hello steps toocreate hello AD application with a password, and assign hello Reader role toohello service principal.</span></span>

1. <span data-ttu-id="64a63-121">Přihlaste se tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="64a63-121">Sign in tooyour account.</span></span>
   
   ```azurecli
   azure login
   ```
2. <span data-ttu-id="64a63-122">toocreate identita aplikace, zadejte název hello aplikace hello a hesla, jak je znázorněno v hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="64a63-122">toocreate an app identity, provide hello name of hello app and a password, as shown in hello following command:</span></span>
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   <span data-ttu-id="64a63-123">Hello nový instanční objekt je vrácen.</span><span class="sxs-lookup"><span data-stu-id="64a63-123">hello new service principal is returned.</span></span> <span data-ttu-id="64a63-124">Hello Id objektu je potřeba při udělování oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64a63-124">hello Object Id is needed when granting permissions.</span></span> <span data-ttu-id="64a63-125">identifikátor guid Hello označené hello hlavní názvy služby je potřeba při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="64a63-125">hello guid listed with hello Service Principal Names is needed when logging in.</span></span> <span data-ttu-id="64a63-126">Tento identifikátor guid je hello stejnou hodnotu jako id aplikace hello. V hello ukázkové aplikace, tato hodnota je hello odkazované tooas `Client ID`.</span><span class="sxs-lookup"><span data-stu-id="64a63-126">This guid is hello same value as hello app id. In hello sample applications, this value is referred tooas hello `Client ID`.</span></span> 
     
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

3. <span data-ttu-id="64a63-127">Udělit oprávnění hello hlavní služby vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="64a63-127">Grant hello service principal permissions on your subscription.</span></span> <span data-ttu-id="64a63-128">V tomto příkladu přidáte hello služby hlavní toohello čtečky role, která uděluje oprávnění tooread všechny prostředky v předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-128">In this example, you add hello service principal toohello Reader role, which grants permission tooread all resources in hello subscription.</span></span> <span data-ttu-id="64a63-129">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="64a63-129">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span> <span data-ttu-id="64a63-130">Pro parametr objectid hello zadejte Id objektu, který jste použili při vytváření aplikace hello hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-130">For hello objectid parameter, provide hello Object Id that you used when creating hello application.</span></span> <span data-ttu-id="64a63-131">Před spuštěním tohoto příkazu, musíte povolit nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="64a63-131">Before running this command, you must allow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="64a63-132">Když ručně spustíte tyto příkazy, obvykle dostatek času uplynul mezi úkoly.</span><span class="sxs-lookup"><span data-stu-id="64a63-132">When you run these commands manually, usually enough time has elapsed between tasks.</span></span> <span data-ttu-id="64a63-133">Ve skriptu, měli byste přidat krok toosleep mezi příkazy hello (jako je `sleep 15`).</span><span class="sxs-lookup"><span data-stu-id="64a63-133">In a script, you should add a step toosleep between hello commands (like `sleep 15`).</span></span> <span data-ttu-id="64a63-134">Pokud uvidíte, že hlavní stanovení hello chyba neexistuje v adresáři hello, znovu spusťte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-134">If you see an error stating hello principal does not exist in hello directory, rerun hello command.</span></span>
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
<span data-ttu-id="64a63-135">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="64a63-135">That's it!</span></span> <span data-ttu-id="64a63-136">Aplikaci AD a instanční objekt se nastavují.</span><span class="sxs-lookup"><span data-stu-id="64a63-136">Your AD application and service principal are set up.</span></span> <span data-ttu-id="64a63-137">Hello další části se dozvíte, jak toolog pomocí hello pověření prostřednictvím rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="64a63-137">hello next section shows you how toolog in with hello credential through Azure CLI.</span></span> <span data-ttu-id="64a63-138">Pokud chcete toouse hello pověření v kódu aplikace, není nutné toocontinue s tímto tématem.</span><span class="sxs-lookup"><span data-stu-id="64a63-138">If you want toouse hello credential in your code application, you do not need toocontinue with this topic.</span></span> <span data-ttu-id="64a63-139">Můžete přeskočit toohello [ukázkové aplikace](#sample-applications) příklady přihlášení pomocí id aplikace a heslo.</span><span class="sxs-lookup"><span data-stu-id="64a63-139">You can jump toohello [Sample applications](#sample-applications) for examples of logging in with your application id and password.</span></span> 

### <a name="provide-credentials-through-azure-cli"></a><span data-ttu-id="64a63-140">Zadejte přihlašovací údaje prostřednictvím rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="64a63-140">Provide credentials through Azure CLI</span></span>
<span data-ttu-id="64a63-141">Nyní musíte toolog v jako operace tooperform aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-141">Now, you need toolog in as hello application tooperform operations.</span></span>

1. <span data-ttu-id="64a63-142">Vždy, když se přihlásíte jako hlavní název služby, musíte pro vaši aplikaci AD id klienta hello tooprovide hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="64a63-142">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="64a63-143">Klient je instance služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="64a63-143">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="64a63-144">id klienta hello tooretrieve pro vaše předplatné aktuálně ověřený, použijte:</span><span class="sxs-lookup"><span data-stu-id="64a63-144">tooretrieve hello tenant id for your currently authenticated subscription, use:</span></span>
   
   ```azurecli
   azure account show
   ```
   
   <span data-ttu-id="64a63-145">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="64a63-145">Which returns:</span></span>
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     <span data-ttu-id="64a63-146">Pokud potřebujete id klienta hello tooget jiného předplatného, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="64a63-146">If you need tooget hello tenant id of another subscription, use hello following command:</span></span>
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. <span data-ttu-id="64a63-147">Pokud potřebujete tooretrieve hello klienta id toouse pro přihlášení, použijte:</span><span class="sxs-lookup"><span data-stu-id="64a63-147">If you need tooretrieve hello client id toouse for logging in, use:</span></span>
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     <span data-ttu-id="64a63-148">Hello hodnota toouse pro přihlášení je guid hello uvedené v hlavní názvy služby hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-148">hello value toouse for logging in is hello guid listed in hello service principal names.</span></span>
   
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
3. <span data-ttu-id="64a63-149">Přihlaste se jako hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="64a63-149">Log in as hello service principal.</span></span>
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    <span data-ttu-id="64a63-150">Zobrazí se výzva k hello hesla.</span><span class="sxs-lookup"><span data-stu-id="64a63-150">You are prompted for hello password.</span></span> <span data-ttu-id="64a63-151">Zadejte heslo hello, kterou jste zadali při vytvoření aplikace hello AD.</span><span class="sxs-lookup"><span data-stu-id="64a63-151">Provide hello password you specified when creating hello AD application.</span></span>
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

<span data-ttu-id="64a63-152">Nyní jste přihlášeni jako hello instanční objekt pro hello instanční objekt, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="64a63-152">You are now authenticated as hello service principal for hello service principal that you created.</span></span>

<span data-ttu-id="64a63-153">Alternativně můžete vyvolat operace REST z příkazového řádku toolog hello v.</span><span class="sxs-lookup"><span data-stu-id="64a63-153">Alternatively, you can invoke REST operations from hello command line toolog in.</span></span> <span data-ttu-id="64a63-154">Z hello odpověď ověřování můžete načíst hello přístupový token pro použití s dalšími operacemi.</span><span class="sxs-lookup"><span data-stu-id="64a63-154">From hello authentication response, you can retrieve hello access token for use with other operations.</span></span> <span data-ttu-id="64a63-155">Příklad získání tokenu přístupu hello vyvoláním operace REST, naleznete v části [generování tokenu přístupu](resource-manager-rest-api.md#generating-an-access-token).</span><span class="sxs-lookup"><span data-stu-id="64a63-155">For an example of retrieving hello access token by invoking REST operations, see [Generating an Access Token](resource-manager-rest-api.md#generating-an-access-token).</span></span>

## <a name="create-service-principal-with-certificate"></a><span data-ttu-id="64a63-156">Vytvoření instančního objektu s certifikátem</span><span class="sxs-lookup"><span data-stu-id="64a63-156">Create service principal with certificate</span></span>
<span data-ttu-id="64a63-157">V této části proveďte postup hello:</span><span class="sxs-lookup"><span data-stu-id="64a63-157">In this section, you perform hello steps to:</span></span>

* <span data-ttu-id="64a63-158">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="64a63-158">create a self-signed certificate</span></span>
* <span data-ttu-id="64a63-159">Vytvoření hello aplikaci AD s hello certifikát a hello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="64a63-159">create hello AD application with hello certificate, and hello service principal</span></span>
* <span data-ttu-id="64a63-160">přiřadit hello čtečky role toohello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="64a63-160">assign hello Reader role toohello service principal</span></span>

<span data-ttu-id="64a63-161">toocomplete tyto kroky, musíte mít [OpenSSL](http://www.openssl.org/) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="64a63-161">toocomplete these steps, you must have [OpenSSL](http://www.openssl.org/) installed.</span></span>

1. <span data-ttu-id="64a63-162">Vytvořte certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="64a63-162">Create a self-signed certificate.</span></span>
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. <span data-ttu-id="64a63-163">Hello předchozím kroku vytvořili dva soubory - privkey.pem a cert.pem.</span><span class="sxs-lookup"><span data-stu-id="64a63-163">hello preceding step created two files - privkey.pem and cert.pem.</span></span> <span data-ttu-id="64a63-164">Veřejné a soukromé klíče hello zkombinovat do jednoho souboru.</span><span class="sxs-lookup"><span data-stu-id="64a63-164">Combine hello public and private keys into a single file.</span></span>

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. <span data-ttu-id="64a63-165">Otevřete hello **examplecert.pem** souborů a vyhledejte hello dlouhé posloupnosti znaků mezi **---BEGIN CERTIFICATE---** a **---END CERTIFICATE---**.</span><span class="sxs-lookup"><span data-stu-id="64a63-165">Open hello **examplecert.pem** file and look for hello long sequence of characters between **-----BEGIN CERTIFICATE-----** and **-----END CERTIFICATE-----**.</span></span> <span data-ttu-id="64a63-166">Zkopírujte data certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-166">Copy hello certificate data.</span></span> <span data-ttu-id="64a63-167">Tato data předat jako parametr při vytváření hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="64a63-167">You pass this data as a parameter when creating hello service principal.</span></span>

4. <span data-ttu-id="64a63-168">Přihlaste se tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="64a63-168">Sign in tooyour account.</span></span>

   ```azurecli
   azure login
   ```
5. <span data-ttu-id="64a63-169">toocreate hello instančního objektu, zadejte název hello hello aplikace a data hello certifikátu, jak je znázorněno v hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="64a63-169">toocreate hello service principal, provide hello name of hello app and hello certificate data, as shown in hello following command:</span></span>
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   <span data-ttu-id="64a63-170">Hello nový instanční objekt je vrácen.</span><span class="sxs-lookup"><span data-stu-id="64a63-170">hello new service principal is returned.</span></span> <span data-ttu-id="64a63-171">Hello Id objektu je potřeba při udělování oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64a63-171">hello Object Id is needed when granting permissions.</span></span> <span data-ttu-id="64a63-172">identifikátor guid Hello označené hello hlavní názvy služby je potřeba při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="64a63-172">hello guid listed with hello Service Principal Names is needed when logging in.</span></span> <span data-ttu-id="64a63-173">Tento identifikátor guid je hello stejnou hodnotu jako id aplikace hello. V hello ukázkové aplikace tato hodnota je odkazované tooas hello ID klienta.</span><span class="sxs-lookup"><span data-stu-id="64a63-173">This guid is hello same value as hello app id. In hello sample applications, this value is referred tooas hello Client ID.</span></span> 
     
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
6. <span data-ttu-id="64a63-174">Udělit oprávnění hello hlavní služby vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="64a63-174">Grant hello service principal permissions on your subscription.</span></span> <span data-ttu-id="64a63-175">V tomto příkladu přidáte hello služby hlavní toohello čtečky role, která uděluje oprávnění tooread všechny prostředky v předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-175">In this example, you add hello service principal toohello Reader role, which grants permission tooread all resources in hello subscription.</span></span> <span data-ttu-id="64a63-176">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="64a63-176">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span> <span data-ttu-id="64a63-177">Pro parametr objectid hello zadejte Id objektu, který jste použili při vytváření aplikace hello hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-177">For hello objectid parameter, provide hello Object Id that you used when creating hello application.</span></span> <span data-ttu-id="64a63-178">Před spuštěním tohoto příkazu, musíte povolit nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="64a63-178">Before running this command, you must allow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="64a63-179">Když ručně spustíte tyto příkazy, obvykle dostatek času uplynul mezi úkoly.</span><span class="sxs-lookup"><span data-stu-id="64a63-179">When you run these commands manually, usually enough time has elapsed between tasks.</span></span> <span data-ttu-id="64a63-180">Ve skriptu, měli byste přidat krok toosleep mezi příkazy hello (jako je `sleep 15`).</span><span class="sxs-lookup"><span data-stu-id="64a63-180">In a script, you should add a step toosleep between hello commands (like `sleep 15`).</span></span> <span data-ttu-id="64a63-181">Pokud uvidíte, že hlavní stanovení hello chyba neexistuje v adresáři hello, znovu spusťte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-181">If you see an error stating hello principal does not exist in hello directory, rerun hello command.</span></span>
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a><span data-ttu-id="64a63-182">Zadejte certifikát pomocí automatizované skriptu rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="64a63-182">Provide certificate through automated Azure CLI script</span></span>
<span data-ttu-id="64a63-183">Nyní musíte toolog v jako operace tooperform aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-183">Now, you need toolog in as hello application tooperform operations.</span></span>

1. <span data-ttu-id="64a63-184">Vždy, když se přihlásíte jako hlavní název služby, musíte pro vaši aplikaci AD id klienta hello tooprovide hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="64a63-184">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="64a63-185">Klient je instance služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="64a63-185">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="64a63-186">id klienta hello tooretrieve pro vaše předplatné aktuálně ověřený, použijte:</span><span class="sxs-lookup"><span data-stu-id="64a63-186">tooretrieve hello tenant id for your currently authenticated subscription, use:</span></span>
   
   ```azurecli
   azure account show
   ```
   
   <span data-ttu-id="64a63-187">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="64a63-187">Which returns:</span></span>
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   <span data-ttu-id="64a63-188">Pokud potřebujete id klienta hello tooget jiného předplatného, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="64a63-188">If you need tooget hello tenant id of another subscription, use hello following command:</span></span>
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. <span data-ttu-id="64a63-189">tooretrieve hello kryptografický otisk certifikátu a odeberte nepotřebné znaky, použijte:</span><span class="sxs-lookup"><span data-stu-id="64a63-189">tooretrieve hello certificate thumbprint and remove unneeded characters, use:</span></span>
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   <span data-ttu-id="64a63-190">Která vrací podobně jako hodnota kryptografického otisku:</span><span class="sxs-lookup"><span data-stu-id="64a63-190">Which returns a thumbprint value similar to:</span></span>
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. <span data-ttu-id="64a63-191">Pokud potřebujete tooretrieve hello klienta id toouse pro přihlášení, použijte:</span><span class="sxs-lookup"><span data-stu-id="64a63-191">If you need tooretrieve hello client id toouse for logging in, use:</span></span>
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   <span data-ttu-id="64a63-192">Hello hodnota toouse pro přihlášení je guid hello uvedené v hlavní názvy služby hello.</span><span class="sxs-lookup"><span data-stu-id="64a63-192">hello value toouse for logging in is hello guid listed in hello service principal names.</span></span>
     
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
4. <span data-ttu-id="64a63-193">Přihlaste se jako hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="64a63-193">Log in as hello service principal.</span></span>
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

<span data-ttu-id="64a63-194">Nyní jste přihlášeni jako hello instanční objekt pro hello aplikaci Azure Active Directory, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="64a63-194">You are now authenticated as hello service principal for hello Azure Active Directory application that you created.</span></span>

## <a name="change-credentials"></a><span data-ttu-id="64a63-195">Změnit pověření</span><span class="sxs-lookup"><span data-stu-id="64a63-195">Change credentials</span></span>

<span data-ttu-id="64a63-196">použít pověření hello toochange pro aplikaci AD, buď kvůli ohrožení zabezpečení nebo vypršení platnosti pověření `azure ad app set`.</span><span class="sxs-lookup"><span data-stu-id="64a63-196">toochange hello credentials for an AD app, either because of a security compromise or a credential expiration, use `azure ad app set`.</span></span>

<span data-ttu-id="64a63-197">toochange heslo, použijte:</span><span class="sxs-lookup"><span data-stu-id="64a63-197">toochange a password, use:</span></span>

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

<span data-ttu-id="64a63-198">toochange hodnotu certifikát, použijte:</span><span class="sxs-lookup"><span data-stu-id="64a63-198">toochange a certificate value, use:</span></span>

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a><span data-ttu-id="64a63-199">Ladění</span><span class="sxs-lookup"><span data-stu-id="64a63-199">Debug</span></span>

<span data-ttu-id="64a63-200">Můžete setkat s následujícím chybám při vytváření objektu služby hello:</span><span class="sxs-lookup"><span data-stu-id="64a63-200">You may encounter hello following errors when creating a service principal:</span></span>

* <span data-ttu-id="64a63-201">**"Authentication_Unauthorized"** nebo **"žádné předplatné nalezena v kontextu hello".**</span><span class="sxs-lookup"><span data-stu-id="64a63-201">**"Authentication_Unauthorized"** or **"No subscription found in hello context."**</span></span> <span data-ttu-id="64a63-202">-Se zobrazí tato chyba, pokud váš účet nemá hello [požadovaná oprávnění](#required-permissions) na Azure Active Directory tooregister hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="64a63-202">- You see this error when your account does not have hello [required permissions](#required-permissions) on hello Azure Active Directory tooregister an app.</span></span> <span data-ttu-id="64a63-203">Obvykle se zobrazí tato chyba při jenom správci ve vašem Azure Active Directory můžete zaregistrovat aplikace a váš účet není správce. Požádejte vašeho správce tooeither přiřadit můžete tooan roli správce, nebo tooenable uživatelé tooregister aplikace.</span><span class="sxs-lookup"><span data-stu-id="64a63-203">Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin. Ask your administrator tooeither assign you tooan administrator role, or tooenable users tooregister apps.</span></span>

* <span data-ttu-id="64a63-204">Váš účet **"nemá autorizace tooperform akce 'Microsoft.Authorization/roleAssignments/write' u rozsahu: /subscriptions/ {guid}'."**  -Zobrazí tato chyba, pokud se váš účet nemá dostatečná oprávnění tooassign identitu tooan role.</span><span class="sxs-lookup"><span data-stu-id="64a63-204">Your account **"does not have authorization tooperform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions tooassign a role tooan identity.</span></span> <span data-ttu-id="64a63-205">Požádejte správce tooadd vaše předplatné můžete tooUser přístupu k roli správce.</span><span class="sxs-lookup"><span data-stu-id="64a63-205">Ask your subscription administrator tooadd you tooUser Access Administrator role.</span></span>

## <a name="sample-applications"></a><span data-ttu-id="64a63-206">Ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="64a63-206">Sample applications</span></span>
<span data-ttu-id="64a63-207">Informace o protokolování jako aplikace hello prostřednictvím různých platforem najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="64a63-207">For information about logging in as hello application through different platforms, see:</span></span>

* [<span data-ttu-id="64a63-208">.NET</span><span class="sxs-lookup"><span data-stu-id="64a63-208">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="64a63-209">Java</span><span class="sxs-lookup"><span data-stu-id="64a63-209">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="64a63-210">Node.js</span><span class="sxs-lookup"><span data-stu-id="64a63-210">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="64a63-211">Python</span><span class="sxs-lookup"><span data-stu-id="64a63-211">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="64a63-212">Ruby</span><span class="sxs-lookup"><span data-stu-id="64a63-212">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a><span data-ttu-id="64a63-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64a63-213">Next steps</span></span>
* <span data-ttu-id="64a63-214">Podrobné pokyny k integraci aplikace do Azure pro správu prostředků najdete v tématu [tooauthorization Příručka pro vývojáře s hello rozhraní API služby Azure Resource Manager](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="64a63-214">For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide tooauthorization with hello Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="64a63-215">Další informace o používání certifikátů a rozhraní příkazového řádku Azure, tooget naleznete v [ověřování prostřednictvím certifikátu s objekty služby Azure z příkazového řádku Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx).</span><span class="sxs-lookup"><span data-stu-id="64a63-215">tooget more information about using certificates and Azure CLI, see [Certificate-based authentication with Azure Service Principals from Linux command line](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx).</span></span> 
* <span data-ttu-id="64a63-216">Seznam dostupných akcí, které může být povolen nebo odepřen toousers najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="64a63-216">For a list of available actions that can be granted or denied toousers, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>
