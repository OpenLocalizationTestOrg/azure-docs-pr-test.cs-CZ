---
title: "Azure Active Directory B2C: Referenční: přizpůsobení hello uživatelského rozhraní cesty uživatele pomocí vlastních zásad | Microsoft Docs"
description: "Téma na Azure Active Directory B2C vlastní zásady"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 11f2a7575b95a186399d83266850fe44d650371b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hello-ui-of-a-user-journey-with-custom-policies"></a>Přizpůsobení hello uživatelského rozhraní cesty uživatele pomocí vlastních zásad

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> Tento článek slouží k pokročilé popis fungování přizpůsobení uživatelského rozhraní a jak tooenable s B2C vlastních zásad, pomocí hello Identity rozhraní Framework


Integrované uživatelské prostředí je klíč pro řešení business k příjemce. Pomocí integrované uživatelské prostředí jsme znamená prostředí, na zařízení nebo prohlížeče, kde cesty uživatele přes naši službu není možné rozlišit od hello zákaznický servis, který používají.

## <a name="understand-hello-cors-way-for-ui-customization"></a>Pochopení hello způsob CORS pro přizpůsobení uživatelského rozhraní

Azure AD B2C umožňuje vám toocustomize hello vzhledu a chování činnost koncového uživatele (UX) na hello stránky, které může potenciálně zpracovat a zobrazit pomocí Azure AD B2C pomocí vlastních zásad.

K tomuto účelu, Azure AD B2C spuštěním kódu v prohlížeči vaše příjemce a používá hello moderní a standard přístup [sdílení prostředků různých původů (CORS)](http://www.w3.org/TR/cors/) tooload vlastní obsah z konkrétní adresy URL, kterou zadáte v vlastní zásady toopoint tooyour HTML5 nebo šablon stylů CSS šablony. CORS je mechanismus, který umožňuje omezené prostředky, jako je písem a na webové stránce toobe, vyžádaný z jiné domény mimo doménu hello, z něhož pochází hello prostředků.

Porovnání toohello staré tradičním způsobem, jakým, kde jsou stránky šablon vlastněny hello řešení, kde poskytuje omezenou textu a obrázků, kde byla omezené řízení rozložení a chování nabízí úvodní toomore než problémy tooachieve integrované prostředí, hello CORS způsob podporuje HTML5 a šablon stylů CSS a umožňují:

- Obsah, hello a hello řešení vloží jeho ovládacích prvků pomocí skript na straně klienta.
- Máte plnou kontrolu nad každý pixel rozložení a chování.

Můžete zadat libovolný počet obsahu stránek jako tím, že vytvoří soubory HTML5 nebo šablon stylů CSS podle potřeby.

> [!NOTE]
> Z bezpečnostních důvodů je aktuálně blokován hello pomocí jazyka JavaScript pro přizpůsobení. je potřeba toounblock JavaScript, použijte vlastní název domény pro vašeho tenanta Azure AD B2C.

V každé z vašich šablon HTML5 nebo šablon stylů CSS je zadat *ukotvení* element, který odpovídá toohello požadované `<div id=”api”>` element hello HTML hello obsahu stránce nebo jako znázornění níže. Azure AD B2C vyžaduje, aby měly všechny stránky obsahu této konkrétní div.

```
<!DOCTYPE html>
<html>
  <head>
    <title>Your page content’s tile!</title>
  </head>
  <body>
    <div id="api"></div>
  </body>
</html>
```

Azure AD B2C související obsah pro stránku hello budou vloženy do této div při hello zbytek stránku hello je váš toocontrol. kód jazyka JavaScript Hello Azure AD B2C vrátí součástí obsahu a vloží do tohoto elementu konkrétní div naše HTML. Azure AD B2C vloží hello následující ovládací prvky podle potřeby: účet výběru ovládacího prvku, přihlášení ovládací prvky, Multi-Factor (aktuálně založené na telefonu) a ovládací prvky kolekce atributů. Azure AD B2C zajišťuje, že všechny ovládací prvky hello HTML5 kompatibilní a přístupné, může plně ve všech ovládacích prvků hello a že nebude vrátit verze ovládacího prvku.

Hello sloučené obsah se nakonec zobrazí hello dynamické dokumentu tooyour příjemce.

tooensure hello výše funguje podle očekávání, musíte:

- Zajistěte, aby byl váš obsah HTML5 kompatibilní a dostupné
- Zajistěte, aby že vaše servery obsahu je povolený pro CORS.
- Poskytovat obsah přes protokol HTTPS.
- Používejte absolutní adresy URL, například https://yourdomain/content pro všechny odkazy a obsah šablon stylů CSS.

> [!TIP]
> tooverify, který hello lokality, které jsou hostiteli obsahu na má povolení CORS a testování požadavků CORS, můžete použít http://test-cors.org/ lokality hello. Děkujeme toothis lokality, můžete jednoduše odeslat hello CORS požadavek tooa na vzdálený server (Pokud je podporováno CORS tootest), nebo odeslat hello CORS požadavku tooa testovací server (tooexplore určité funkce CORS).

> [!TIP]
> Hello lokality http://enable-cors.org/ představuje také více než užitečné zdroje na CORS.

Thanks toothis přístup založený na CORS hello koncoví uživatelé pak bude mít konzistentního prostředí mezi vaší aplikace a stránky hello obsluhuje Azure AD B2C.

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Předpokladem je je nutné toocreate účet úložiště. Budete potřebovat předplatnému Azure toocreate účet Azure Blob Storage. Můžete si zaregistrovat bezplatnou zkušební verzi na hello [webu Azure](https://azure.microsoft.com/en-us/pricing/free-trial/).

1. Spustí relaci prohlížeče a přejděte toohello [portál Azure](https://portal.azure.com).
2. Přihlaste se pomocí přihlašovacích údajů správce.
3. Klikněte na tlačítko **nový** > **Data + úložiště** > **účet úložiště**.  A **vytvořit účet úložiště** otevře se okno.
4. V **název**, zadejte název pro účet úložiště hello, například *contoso369b2c*. Tuto hodnotu budou později označovaná jako příliš*storageAccountName*.
5. Vyberte odpovídající možnosti hello pro hello cenová úroveň, skupinu prostředků hello a hello předplatného. Ujistěte se, že máte hello **Pin tooStartboard** možnost zaškrtnutí. Klikněte na možnost **Vytvořit**.
6. Přejděte zpět toohello úvodním panelu a klikněte na účet úložiště hello, kterou jste právě vytvořili.
7. V hello **služby** klikněte na tlačítko **objekty BLOB**. A **okno služby objektů Blob** otevře.
8. Klikněte na tlačítko **+ kontejner**.
9. V **název**, zadejte název pro hello kontejneru, například *b2c*. Tato hodnota bude později odkazované tooas *containerName*.
9. Vyberte **Blob** jako hello **přistupovat typu**. Klikněte na možnost **Vytvořit**.
10. Hello kontejneru, který jste vytvořili se zobrazí v seznamu hello na hello **okno služby objektů Blob**.
11. Zavřít hello **objekty BLOB** okno.
12. Na hello **okně účtu úložiště**, klikněte na tlačítko hello **klíč** ikonu. **Okna klíče přístup** otevře.  
13. Poznamenejte si hodnotu hello **key1**. Tuto hodnotu budou později označovaná jako *key1*.

## <a name="downloading-hello-helper-tool"></a>Stahování pomocným nástrojem pro hello

1.  Stáhněte si nástroj Pomocník hello z [Githubu](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).
2.  Uložit hello *B2C-AzureBlobStorage-klienta master.zip* soubor na místním počítači.
3.  Extrahování obsahu hello hello B2C-AzureBlobStorage-klienta master.zip souboru na váš místní disk, například v rámci hello **sady přizpůsobení uživatelského rozhraní** složky. Tím se vytvoří *B2C AzureBlobStorage klienta hlavní* složky pod.
4.  Otevřete tuto složku a extrahování obsahu hello souboru archivu hello *B2CAzureStorageClient.zip* v něm.

## <a name="upload-hello-ui-customization-pack-sample-files"></a>Odeslání ukázkových souborů hello sady přizpůsobení uživatelského rozhraní

1.  Pomocí Průzkumníka Windows přejděte složky toohello *B2C AzureBlobStorage klienta hlavní* přidávaném hello *sady přizpůsobení uživatelského rozhraní* složky vytvořené v předchozí části hello.
2.  Spustit hello *B2CAzureStorageClient.exe* souboru. Tento program bude jednoduše nahrát všechny hello soubory v adresáři hello zadejte účet úložiště tooyour a povolení CORS přístup pro tyto soubory.
3.  Po zobrazení výzvy zadejte:.  název účtu úložiště Hello *storageAccountName*, například *contoso369b2c*.
    b.  Hello primární přístupový klíč úložiště objektů blob v azure, *key1*, například *contoso369b2c*.
    c.  Hello název vašeho kontejneru úložiště objektů blob úložiště *containerName*, například *b2c*.
    d.  Cesta Hello hello *Starter-Pack* ukázkové soubory, například *... \B2CTemplates\wingtiptoys*.

Pokud jste postupovali podle kroků hello výše, hello jazyka HTML5 a šablon stylů CSS soubory hello *sady přizpůsobení uživatelského rozhraní* pro fiktivní společnosti hello **Northwind** bude nyní odkazující tooyour účet úložiště.  Můžete ověřit, že obsah hello se nahrají správně tak, že otevřete okno hello související kontejneru v hello portálu Azure. Případně můžete ověřit, že hello obsah se nahrají správně pomocí přístupu na stránku hello z prohlížeče. Další informace najdete v tématu [Azure Active Directory B2C: přizpůsobení funkce uživatelského rozhraní (UI) toodemonstrate hello stránky použít pomocný nástroj](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-hello-storage-account-has-cors-enabled"></a>Zajistěte, aby byl účet úložiště hello povolení CORS

CORS (sdílení prostředků různého původu) musí být povolená na váš koncový bod pro Azure AD B2C Premium tooload svůj obsah. Je to proto, že váš obsah uložený v jiné doméně než hello domény Premium Azure AD B2C bude obsluhující stránku hello z.

tooverify, který hello úložiště, které jsou hostiteli obsahu na má CORS povoleno, pokračujte hello následující kroky:

1. Spustí relaci prohlížeče a přejděte na stránku toohello *unified.html* pomocí hello úplnou adresu URL jeho umístění ve vašem účtu úložiště `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Například https://contoso369b2c.blob.core.windows.net/b2c/unified.html.
2. Přejděte toohttp://test-cors.org. Tento web umožní, že jste tooverify který hello stránku, kterou používáte má povolení CORS.  
<!--
![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
-->

3. V **vzdálenou adresou URL**zadejte úplnou adresu URL hello unified.html obsah a klikněte na tlačítko **odeslat požadavek**.
4. Ověřte, že výstup hello v hello **výsledky** část obsahuje *XHR stav: 200*. To znamená, že je povolena CORS.
<!--
![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
-->
účet úložiště Hello by měl obsahovat teď kontejner objektů blob s názvem *b2c* naše obrázku, který obsahuje hello následujících Northwind šablony z hello *Starter-Pack*.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

Hello následující tabulka popisuje účel hello hello výše stránkách HTML5.

| HTML5 šablony | Popis |
|----------------|-------------|
| *phonefactor.HTML* | Tato stránka slouží jako šablona pro stránku služby Multi-Factor authentication. |
| *ResetPassword.HTML* | Tato stránka slouží jako šablona pro zapomněli jste heslo?. |
| *selfasserted.HTML* | Tato stránka slouží jako šablona pro stránku pro přihlášení sociálních účet, stránku pro přihlášení místní účet nebo na přihlašovací stránku místní účet. |
| *Unified.HTML* | Tato stránka slouží jako šablona pro jednotné stránku registrace nebo přihlášení. |
| *updateprofile.HTML* | Tato stránka slouží jako šablona pro stránku aktualizace profilu. |

## <a name="add-a-link-tooyour-html5css-templates-tooyour-user-journey"></a>Přidat odkaz tooyour HTML5 nebo šablon stylů CSS šablony tooyour uživatele cesty

Můžete přidat odkaz tooyour HTML5 nebo šablon stylů CSS šablony tooyour uživatele cesty přímou úpravou vlastní zásadu.

Hello vlastní HTML5 nebo šablon stylů CSS šablony toouse v vám dobře slouží uživatele mít toobe zadané v seznamu obsahu definice, které lze použít v těchto cesty uživatele. K tomuto účelu, volitelný  *<ContentDefinitions>*  – element XML musí být deklarován v části hello  *<BuildingBlocks>*  části souboru XML vlastní zásady.

Hello následující tabulka popisuje hello sadu rozpoznáno stroj prostředí identity hello Azure AD B2C a hello typ stránky, která má vztah toothem ID obsahu definice.

| Id obsahu definice | Popis |
|-----------------------|-------------|
| *API.Error* | **Chybové stránky**. Tato stránka se zobrazí, když je došlo k výjimce nebo došlo k chybě. |
| *API.idpselections* | **Stránka Výběr zprostředkovatele identity**. Tato stránka obsahuje seznam identity, které můžete zvolit zprostředkovatelé, kteří hello uživatele při přihlášení. Jedná se buď poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google +, nebo místní účty (podle e-mailové adresy nebo uživatelské jméno). |
| *API.idpselections.Signup* | **Výběr zprostředkovatele identity pro registraci**. Tato stránka obsahuje seznam identity, které můžete zvolit zprostředkovatelé, kteří hello uživatele během registrace. Jedná se buď poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google +, nebo místní účty (podle e-mailové adresy nebo uživatelské jméno). |
| *API.localaccountpasswordreset* | **Zapomněli jste heslo**. Tato stránka obsahuje formuláře hello má toofill tooinitiate resetovat své heslo.  |
| *API.localaccountsignin* | **Přihlašovací stránka místní účet**. Tato stránka obsahuje formuláře přihlášení tohoto uživatele hello má toofill v při přihlášení pomocí místního účtu, který je založený na e-mailovou adresu nebo uživatelské jméno. Hello formulář může obsahovat vstupní textové pole a pole pro zadání hesla. |
| *API.localaccountsignup* | **Místní účet stránku**. Tato stránka obsahuje registračního formuláře hello má toofill při registraci pro místní účet, který je založený na e-mailovou adresu nebo uživatelské jméno. Hello formulář může obsahovat různé vstupní ovládací prvky jako vstupní textové pole, pole pro zadání hesla, přepínač, polí rozevíracího seznamu vyberte jeden a více vyberte zaškrtávací políčka. |
| *API.phonefactor* | **Stránka služby Multi-Factor authentication**. Na této stránce si uživatelé mohli ověřit jejich telefonních čísel (pomocí textové nebo hlasové) během registrace nebo přihlášení. |
| *API.selfasserted* | **Stránku pro přihlášení sociálních účet**. Tato stránka obsahuje registračního formuláře hello má toofill v při registraci pomocí existující účet od poskytovatele identity sociálních třeba Facebook nebo Google +. Tato stránka je podobné toohello nad stránku pro přihlášení sociálních účet s výjimkou hello hello pole zadání hesla. |
| *API.selfasserted.profileupdate* | **Stránka pro aktualizaci profilu**. Tato stránka obsahuje formuláře se hello může pomocí tooupdate svůj profil. Tato stránka je podobné toohello nad stránku pro přihlášení sociálních účet s výjimkou hello hello pole zadání hesla. |
| *API.signuporsignin* | **Jednotná stránku registrace nebo přihlášení**.  Tato stránka zpracovává jak registrace a přihlášení uživatelů, kteří můžou využívat poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook nebo Google + nebo místní účty.

## <a name="next-steps"></a>Další kroky
[Referenční dokumentace: Pochopit, jak vlastních zásad práce s hello Identity Framework prostředí v B2C](active-directory-b2c-reference-custom-policies-understanding-contents.md)
