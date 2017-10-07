---
title: "aaaConfigure vlastního názvu doménu pro koncový bod služby Azure Blob storage | Microsoft Docs"
description: "Pomocí hello Azure portálu toomap koncový bod úložiště vlastní název v kanonickém tvaru (CNAME) toohello objektů Blob v účtu Azure Storage."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: aaafd8c5-eacb-49dc-8c8b-3f7011ad5e92
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: marsma
ms.openlocfilehash: bfee6d28039f389ba2e2ed6b28d43894b6e15469
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Konfigurace vlastního názvu domény pro koncový bod služby Blob Storage

Můžete nakonfigurovat vlastní doménu pro přístup k datům objektu blob v účtu úložiště Azure. Hello výchozí koncový bod pro úložiště objektů Blob je `<storage-account-name>.blob.core.windows.net`. Pokud namapujete vlastních domén a subdomén jako **www.contoso.com** toohello koncový bod objektů blob pro váš účet úložiště, vaši uživatelé můžete poté přistoupit blob data ve vašem účtu úložiště pomocí této domény.

> [!IMPORTANT]
> Úložiště Azure ještě nativně nepodporuje protokol HTTPS s vlastní domény. Můžete v danou chvíli [používat objekty BLOB tooaccess Azure CDN hello s vlastní domény prostřednictvím protokolu HTTPS](storage-https-custom-domain-cdn.md).
>

Hello následující tabulka uvádí několik ukázkových adres URL pro data objektů blob v účtu úložiště s názvem **můj_účet_úložiště**. vlastní doména Hello zaregistrovaná pro účet úložiště hello je **www.contoso.com**:

| Typ prostředku | Výchozí adresa URL | Adresa URL vlastní domény |
| --- | --- | --- |
| Účet úložiště | http://mystorageaccount.BLOB.Core.Windows.NET | http://www.contoso.com |
| Objekt blob |http://mystorageaccount.BLOB.Core.Windows.NET/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Kořenový kontejner | http://mystorageaccount.BLOB.Core.Windows.NET/myblob nebo http://mystorageaccount.blob.core.windows.net/$ kořenové nebo můj_objekt_blob| http://www.contoso.com/myblob nebo http://www.contoso.com/$ kořenové nebo můj_objekt_blob |

## <a name="direct-vs-intermediary-domain-mapping"></a>Přímé oproti mapování zprostředkující domény

Existují dva způsoby toopoint vlastní domény toohello koncový bod služby blob pro váš účet úložiště: přímé CNAME mapování a pomocí hello *asverify* zprostředkující subdomény.

### <a name="direct-cname-mapping"></a>Přímé mapování CNAME

první a nejjednodušší, metoda Hello je toocreate záznam název v kanonickém tvaru (CNAME), který mapuje vaše vlastní domény a subdomény přímo toohello blob koncový bod. Záznam CNAME je funkce system (DNS) název domény, který se mapuje zdrojové domény tooa cílové domény. V takovém případě hello zdrojovou doménu je vlastní vlastní domény a subdomény, například *www.contoso.com*. hello cílová doména je koncový bod služby objektů Blob, například  *mystorageaccount.BLOB.Core.Windows.NET*.

Přímá metoda Hello je popsaná v [zaregistrovat vlastní doménu](#register-a-custom-domain).

### <a name="intermediary-mapping-with-asverify"></a>Zprostředkující mapování s *asverify*

druhý metoda Hello používá také záznamy CNAME, ale nejdřív využívá speciální subdoména rozpoznáno výpadek Azure tooavoid: **asverify**.

Hello proces mapování koncový bod služby vlastní domény tooa blob může způsobit na krátkou dobu výpadku pro doménu hello jeho registrace v hello [portál Azure](https://portal.azure.com). Pokud vaše vlastní doména je aktuálně podporuje aplikace s dohodu o úrovni služeb (SLA) vyžadující nepřeruší, pak můžete použít hello Azure *asverify* subdomény jako zprostředkující registrace krok. Tento zprostředkující krok zajistí uživatelům je možné tooaccess doménu, zatímco probíhá mapování DNS hello.

Hello zprostředkující metoda je popsaná v [zaregistrovat vlastní doménu pomocí hello *asverify* subdomény](#register-a-custom-domain-using-the-asverify-subdomain).

## <a name="register-a-custom-domain"></a>Zaregistrovat k vlastní doméně
Použijte tento postup tooregister vaši vlastní doménu, pokud máte obavu, informace o doméně hello se stručně není k dispozici tooyour uživatelé, nebo pokud vaše vlastní doména není aktuálně hostování aplikace.

Pokud vaše vlastní doména je aktuálně podporuje aplikace, která nemůže mít žádné výpadky, postupujte podle uvedených v postupu hello [zaregistrovat vlastní doménu pomocí hello *asverify* subdomény](#register-a-custom-domain-using-the-asverify-subdomain).

tooconfigure vlastní název domény, musíte vytvořit nový záznam CNAME ve službě DNS. Hello záznam CNAME Určuje alias pro název domény. V takovém případě mapuje hello adresu vlastní domény toohello úložiště koncový bod služby Blob pro váš účet úložiště.

Obvykle můžete spravovat nastavení vaše doména DNS na webu registrátora domény. Každý Registrátor má podobné, ale poněkud liší metodu zadávání záznam CNAME, ale koncept hello je hello stejné. Některé balíčky registrace základní domény nenabízejí konfiguraci DNS, může být nutné tooupgrade vaší domény registrační balíček než bude možné vytvořit záznam CNAME hello.

1. Přejděte tooyour účet úložiště v hello [portál Azure](https://portal.azure.com).
1. V části **služby objektů BLOB** v okně hello nabídky, vyberte **vlastní domény** tooopen hello *vlastní domény* okno.
1. Přihlaste se na webu registrátora domény tooyour a přejděte toohello stránku pro správu DNS. Může to být v části, jako je **Název domény**, **DNS** nebo **Správa názvového serveru**.
1. Najít oddíl hello správy záznamů CNAME. Můžete mít toogo tooan upřesňující nastavení stránku a vyhledejte slova hello **CNAME**, **Alias**, nebo **subdomény**.
1. Vytvořit nový záznam CNAME a zadejte alias subdomény, jako **www** nebo **fotografie**. Potom zadejte název hostitele, který je váš koncový bod objektu Blob služby, ve formátu hello **mystorageaccount.blob.core.windows.net** (kde *můj_účet_úložiště* je hello název účtu úložiště). toouse název hostitele Hello se zobrazí v položce #1 hello *vlastní domény* okno v hello [portál Azure](https://portal.azure.com).
1. Do textového pole hello na hello *vlastní domény* okno v hello [portál Azure](https://portal.azure.com), zadejte název vlastní domény, včetně hello subdomény hello. Například, pokud vaše doména je **contoso.com** a je váš alias subdomény **www**, zadejte **www.contoso.com**. Pokud je vaše subdomény **fotografie**, zadejte **photos.contoso.com**. subdomény hello je *požadované*.
1. Vyberte **Uložit** na hello *vlastní domény* okno tooregister vaši vlastní doménu. Pokud registrace hello je úspěšné, zobrazí se portálu oznámení, že váš účet úložiště se úspěšně aktualizoval.

Jakmile váš nový záznam CNAME se rozšíří přes DNS, vaši uživatelé můžete zobrazit data objektu blob pomocí vlastní domény, tak dlouho, dokud mají hello příslušná oprávnění.

## <a name="register-a-custom-domain-using-hello-asverify-subdomain"></a>Zaregistrovat vlastní doménu pomocí hello *asverify* subdomény
Použijte tento postup tooregister vaši vlastní doménu. Pokud vaše vlastní doména je aktuálně podporujete aplikace s SLA, která vyžaduje, aby se stát, že nedojde k žádnému výpadku. Vytvořit záznam CNAME, který odkazuje z `asverify.<subdomain>.<customdomain>` příliš`asverify.<storageaccount>.blob.core.windows.net`, můžete předem zaregistrovat doménu s Azure. Potom můžete vytvořit druhý CNAME, který odkazuje z `<subdomain>.<customdomain>` příliš`<storageaccount>.blob.core.windows.net`, okamžiku provoz tooyour vlastní doména bude koncový bod směrovanou tooyour objektů blob.

Hello **asverify** subdomény je speciální subdoména rozpoznáno Azure. Předponou `asverify` tooyour vlastní subdomény, můžete povolit Azure toorecognize vaši vlastní doménu beze změny hello záznam DNS pro doménu hello. Když upravíte hello záznam DNS pro doménu hello, bude koncový bod objektů blob namapované toohello bez výpadků.

1. Přejděte tooyour účet úložiště v hello [portál Azure](https://portal.azure.com).
1. V části **služby objektů BLOB** v okně hello nabídky, vyberte **vlastní domény** tooopen hello *vlastní domény* okno.
1. Přihlaste se na poskytovatele DNS tooyour webu a přejděte toohello stránku pro správu DNS. Může to být v části, jako je **Název domény**, **DNS** nebo **Správa názvového serveru**.
1. Najít oddíl hello správy záznamů CNAME. Můžete mít toogo tooan upřesňující nastavení stránku a vyhledejte slova hello **CNAME**, **Alias**, nebo **subdomény**.
1. Vytvořit nový záznam CNAME a zadejte alias subdomény, která zahrnuje hello *asverify* subdomény. Například **asverify.www** nebo **asverify.photos**. Potom zadejte název hostitele, který je váš koncový bod objektu Blob služby, ve formátu hello **asverify.mystorageaccount.blob.core.windows.net** (kde **můj_účet_úložiště** je hello název účtu úložiště). toouse název hostitele Hello se zobrazí v položce #2 hello *vlastní domény* okno v hello [portál Azure](https://portal.azure.com).
1. Do textového pole hello na hello *vlastní domény* okno v hello [portál Azure](https://portal.azure.com), zadejte název vlastní domény, včetně hello subdomény hello. Nezahrnovat *asverify*. Například, pokud vaše doména je **contoso.com** a je váš alias subdomény **www**, zadejte **www.contoso.com**. Pokud je vaše subdomény **fotografie**, zadejte **photos.contoso.com**. subdomény hello je vyžadován.
1. Vyberte hello **použít nepřímé ověření CNAME** zaškrtávací políčko.
1. Vyberte **Uložit** na hello *vlastní domény* okno tooregister vaši vlastní doménu. Pokud registrace hello je úspěšné, zobrazí se portálu oznámení s informacemi o tom, že váš účet úložiště se úspěšně aktualizoval. V tomto okamžiku vaše vlastní doména byla ověřena Azure, ale provoz tooyour doména není zatím směrovány tooyour účet úložiště.
1. Vrátí poskytovatele DNS tooyour webu a vytvořit další záznam CNAME, který se mapuje koncový bod služby objektů Blob tooyour subdomény. Můžete například zadat hello subdomény jako **www** nebo **fotografie** (bez hello *asverify*), a název hostitele jako hello  **mystorageaccount.BLOB.Core.Windows.NET** (kde **můj_účet_úložiště** je hello název účtu úložiště). Pro tento krok je dokončena registrace hello vaši vlastní doménu.
1. Nakonec můžete odstranit záznam CNAME hello jste vytvořili obsahující hello **asverify** subdomény, protože byla potřeba pouze jako zprostředkující krok.

Jakmile váš nový záznam CNAME se rozšíří přes DNS, vaši uživatelé můžete zobrazit data objektu blob pomocí vlastní domény, tak dlouho, dokud mají hello příslušná oprávnění.

## <a name="test-your-custom-domain"></a>Otestovat vaši vlastní doménu.

Vytvořte objekt blob ve veřejném kontejneru v rámci účtu úložiště, tooconfirm, které vaše vlastní doména je skutečně namapován tooyour koncový bod služby objektů Blob. Ve webovém prohlížeči, používání identifikátoru URI v hello následující formát tooaccess hello blob:

`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Například můžete použít následující tooaccess URI webového formuláře v hello hello **myforms** kontejneru v hello **photos.contoso.com** vlastní subdomény:

`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Zrušení registrace k vlastní doméně

tooderegister vlastní doménu pro koncový bod úložiště objektů Blob, použijte jednu z následujících postupů hello.

### <a name="azure-portal"></a>portál Azure

Proveďte následující hello v nastavení vlastní domény hello Azure portálu tooremove hello:

1. Přejděte tooyour účet úložiště v hello [portál Azure](https://portal.azure.com).
1. V části **služby objektů BLOB** v okně hello nabídky, vyberte **vlastní domény** tooopen hello *vlastní domény* okno.
1. Vymazat hello obsah hello textového pole obsahující vlastní název domény.
1. Vyberte hello **Uložit** tlačítko.

Při vlastní domény hello byl úspěšně odebrán, zobrazí se portálu oznámení s informacemi o tom, že váš účet úložiště se úspěšně aktualizoval.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Použití hello [aktualizace účtu úložiště az](https://docs.microsoft.com/cli/azure/storage/account#update) rozhraní příkazového řádku příkaz a zadat prázdný řetězec (`""`) pro hello `--custom-domain` argument hodnotu tooremove registrace vlastní domény.

* Formát příkazu:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Příkaz příklad:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

Použití hello [Set AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) rutiny prostředí PowerShell a zadat prázdný řetězec (`""`) pro hello `-CustomDomainName` argument hodnotu tooremove registrace vlastní domény.

* Formát příkazu:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Příkaz příklad:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Další kroky
* [Mapování koncový bod vlastní domény tooan Azure Content Delivery Network (CDN)](../../cdn/cdn-map-content-to-custom-domain.md)
* [Pomocí vlastních domén objekty BLOB tooaccess hello Azure CDN prostřednictvím protokolu HTTPS](storage-https-custom-domain-cdn.md)
