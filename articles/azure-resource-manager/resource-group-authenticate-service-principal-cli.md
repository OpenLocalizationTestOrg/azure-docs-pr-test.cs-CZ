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
# <a name="use-azure-cli-toocreate-a-service-principal-tooaccess-resources"></a>Pomocí rozhraní příkazového řádku Azure toocreate hlavní tooaccess prostředky služby

Pokud máte aplikace nebo skript, který potřebuje tooaccess prostředků, můžete nastavit identity aplikace hello a ověřit hello aplikaci s svoje vlastní přihlašovací údaje. Tato identita se označuje jako hlavní název služby. Tento přístup umožňuje:

* Přiřazení oprávnění toohello identita aplikace, která se liší od vlastní oprávnění. Obvykle se tato oprávnění se s omezeným přístupem tooexactly jaké aplikace hello musí toodo.
* Při provádění bezobslužného skriptu, použijte certifikát pro ověřování.

Tento článek ukazuje, jak toouse [Azure CLI 1.0](../cli-install-nodejs.md) tooset až toorun aplikaci v části vlastní pověření a identity. Nainstalujte nejnovější verzi k hello [Azure CLI 1.0](../cli-install-nodejs.md) toomake prostředí, která odpovídá hello příklady v tomto článku.

## <a name="required-permissions"></a>Požadovaná oprávnění
toocomplete tohoto tématu, musíte mít dostatečná oprávnění v Azure Active Directory a vašeho předplatného Azure. Konkrétně musí být schopný toocreate aplikace v hello Azure Active Directory a přiřadit role hlavní tooa služby hello. 

Hello bylo jestli váš účet má odpovídající oprávnění je prostřednictvím portálu hello nejjednodušší toocheck způsobem. Informace najdete v článku [Kontrola požadovaných oprávnění na portálu](resource-group-create-service-principal-portal.md#required-permissions).

Nyní, pokračovat tooa části pro buď [heslo](#create-service-principal-with-password) nebo [certifikát](#create-service-principal-with-certificate) ověřování.

## <a name="create-service-principal-with-password"></a>Vytvoření instančního objektu s heslem
V této části provést hello kroky toocreate hello aplikaci AD s heslem a přiřaďte hello čtečky role toohello instanční objekt.

1. Přihlaste se tooyour účtu.
   
   ```azurecli
   azure login
   ```
2. toocreate identita aplikace, zadejte název hello aplikace hello a hesla, jak je znázorněno v hello následující příkaz:
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   Hello nový instanční objekt je vrácen. Hello Id objektu je potřeba při udělování oprávnění. identifikátor guid Hello označené hello hlavní názvy služby je potřeba při přihlášení. Tento identifikátor guid je hello stejnou hodnotu jako id aplikace hello. V hello ukázkové aplikace, tato hodnota je hello odkazované tooas `Client ID`. 
     
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

3. Udělit oprávnění hello hlavní služby vaše předplatné. V tomto příkladu přidáte hello služby hlavní toohello čtečky role, která uděluje oprávnění tooread všechny prostředky v předplatném hello. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md). Pro parametr objectid hello zadejte Id objektu, který jste použili při vytváření aplikace hello hello. Před spuštěním tohoto příkazu, musíte povolit nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory. Když ručně spustíte tyto příkazy, obvykle dostatek času uplynul mezi úkoly. Ve skriptu, měli byste přidat krok toosleep mezi příkazy hello (jako je `sleep 15`). Pokud uvidíte, že hlavní stanovení hello chyba neexistuje v adresáři hello, znovu spusťte příkaz hello.
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
A to je vše! Aplikaci AD a instanční objekt se nastavují. Hello další části se dozvíte, jak toolog pomocí hello pověření prostřednictvím rozhraní příkazového řádku Azure. Pokud chcete toouse hello pověření v kódu aplikace, není nutné toocontinue s tímto tématem. Můžete přeskočit toohello [ukázkové aplikace](#sample-applications) příklady přihlášení pomocí id aplikace a heslo. 

### <a name="provide-credentials-through-azure-cli"></a>Zadejte přihlašovací údaje prostřednictvím rozhraní příkazového řádku Azure
Nyní musíte toolog v jako operace tooperform aplikace hello.

1. Vždy, když se přihlásíte jako hlavní název služby, musíte pro vaši aplikaci AD id klienta hello tooprovide hello adresáře. Klient je instance služby Azure Active Directory. id klienta hello tooretrieve pro vaše předplatné aktuálně ověřený, použijte:
   
   ```azurecli
   azure account show
   ```
   
   Která vrací:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     Pokud potřebujete id klienta hello tooget jiného předplatného, použijte následující příkaz hello:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Pokud potřebujete tooretrieve hello klienta id toouse pro přihlášení, použijte:
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     Hello hodnota toouse pro přihlášení je guid hello uvedené v hlavní názvy služby hello.
   
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
3. Přihlaste se jako hello instanční objekt.
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    Zobrazí se výzva k hello hesla. Zadejte heslo hello, kterou jste zadali při vytvoření aplikace hello AD.
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

Nyní jste přihlášeni jako hello instanční objekt pro hello instanční objekt, který jste vytvořili.

Alternativně můžete vyvolat operace REST z příkazového řádku toolog hello v. Z hello odpověď ověřování můžete načíst hello přístupový token pro použití s dalšími operacemi. Příklad získání tokenu přístupu hello vyvoláním operace REST, naleznete v části [generování tokenu přístupu](resource-manager-rest-api.md#generating-an-access-token).

## <a name="create-service-principal-with-certificate"></a>Vytvoření instančního objektu s certifikátem
V této části proveďte postup hello:

* Vytvořit certifikát podepsaný svým držitelem
* Vytvoření hello aplikaci AD s hello certifikát a hello instančního objektu
* přiřadit hello čtečky role toohello instančního objektu

toocomplete tyto kroky, musíte mít [OpenSSL](http://www.openssl.org/) nainstalována.

1. Vytvořte certifikát podepsaný svým držitelem.
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. Hello předchozím kroku vytvořili dva soubory - privkey.pem a cert.pem. Veřejné a soukromé klíče hello zkombinovat do jednoho souboru.

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. Otevřete hello **examplecert.pem** souborů a vyhledejte hello dlouhé posloupnosti znaků mezi **---BEGIN CERTIFICATE---** a **---END CERTIFICATE---**. Zkopírujte data certifikátu hello. Tato data předat jako parametr při vytváření hello instanční objekt.

4. Přihlaste se tooyour účtu.

   ```azurecli
   azure login
   ```
5. toocreate hello instančního objektu, zadejte název hello hello aplikace a data hello certifikátu, jak je znázorněno v hello následující příkaz:
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   Hello nový instanční objekt je vrácen. Hello Id objektu je potřeba při udělování oprávnění. identifikátor guid Hello označené hello hlavní názvy služby je potřeba při přihlášení. Tento identifikátor guid je hello stejnou hodnotu jako id aplikace hello. V hello ukázkové aplikace tato hodnota je odkazované tooas hello ID klienta. 
     
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
6. Udělit oprávnění hello hlavní služby vaše předplatné. V tomto příkladu přidáte hello služby hlavní toohello čtečky role, která uděluje oprávnění tooread všechny prostředky v předplatném hello. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md). Pro parametr objectid hello zadejte Id objektu, který jste použili při vytváření aplikace hello hello. Před spuštěním tohoto příkazu, musíte povolit nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory. Když ručně spustíte tyto příkazy, obvykle dostatek času uplynul mezi úkoly. Ve skriptu, měli byste přidat krok toosleep mezi příkazy hello (jako je `sleep 15`). Pokud uvidíte, že hlavní stanovení hello chyba neexistuje v adresáři hello, znovu spusťte příkaz hello.
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a>Zadejte certifikát pomocí automatizované skriptu rozhraní příkazového řádku Azure
Nyní musíte toolog v jako operace tooperform aplikace hello.

1. Vždy, když se přihlásíte jako hlavní název služby, musíte pro vaši aplikaci AD id klienta hello tooprovide hello adresáře. Klient je instance služby Azure Active Directory. id klienta hello tooretrieve pro vaše předplatné aktuálně ověřený, použijte:
   
   ```azurecli
   azure account show
   ```
   
   Která vrací:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   Pokud potřebujete id klienta hello tooget jiného předplatného, použijte následující příkaz hello:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. tooretrieve hello kryptografický otisk certifikátu a odeberte nepotřebné znaky, použijte:
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   Která vrací podobně jako hodnota kryptografického otisku:
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. Pokud potřebujete tooretrieve hello klienta id toouse pro přihlášení, použijte:
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   Hello hodnota toouse pro přihlášení je guid hello uvedené v hlavní názvy služby hello.
     
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
4. Přihlaste se jako hello instanční objekt.
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

Nyní jste přihlášeni jako hello instanční objekt pro hello aplikaci Azure Active Directory, kterou jste vytvořili.

## <a name="change-credentials"></a>Změnit pověření

použít pověření hello toochange pro aplikaci AD, buď kvůli ohrožení zabezpečení nebo vypršení platnosti pověření `azure ad app set`.

toochange heslo, použijte:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

toochange hodnotu certifikát, použijte:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a>Ladění

Můžete setkat s následujícím chybám při vytváření objektu služby hello:

* **"Authentication_Unauthorized"** nebo **"žádné předplatné nalezena v kontextu hello".** -Se zobrazí tato chyba, pokud váš účet nemá hello [požadovaná oprávnění](#required-permissions) na Azure Active Directory tooregister hello aplikace. Obvykle se zobrazí tato chyba při jenom správci ve vašem Azure Active Directory můžete zaregistrovat aplikace a váš účet není správce. Požádejte vašeho správce tooeither přiřadit můžete tooan roli správce, nebo tooenable uživatelé tooregister aplikace.

* Váš účet **"nemá autorizace tooperform akce 'Microsoft.Authorization/roleAssignments/write' u rozsahu: /subscriptions/ {guid}'."**  -Zobrazí tato chyba, pokud se váš účet nemá dostatečná oprávnění tooassign identitu tooan role. Požádejte správce tooadd vaše předplatné můžete tooUser přístupu k roli správce.

## <a name="sample-applications"></a>Ukázkové aplikace
Informace o protokolování jako aplikace hello prostřednictvím různých platforem najdete v tématu:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Další kroky
* Podrobné pokyny k integraci aplikace do Azure pro správu prostředků najdete v tématu [tooauthorization Příručka pro vývojáře s hello rozhraní API služby Azure Resource Manager](resource-manager-api-authentication.md).
* Další informace o používání certifikátů a rozhraní příkazového řádku Azure, tooget naleznete v [ověřování prostřednictvím certifikátu s objekty služby Azure z příkazového řádku Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
* Seznam dostupných akcí, které může být povolen nebo odepřen toousers najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).
