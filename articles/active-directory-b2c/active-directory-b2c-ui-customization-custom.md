---
title: "Přizpůsobení uživatelského rozhraní pomocí vlastních zásad – Azure AD B2C | Microsoft Docs"
description: "Další informace o přizpůsobení uživatelského rozhraní (UI), zatímco pomocí vlastních zásad v Azure AD B2C."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 6f00995e54c9f9ef27cc51e38f3de07cd5817cc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a>Azure Active Directory B2C: Konfigurace ve vlastních zásadách pro přizpůsobení uživatelského rozhraní

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Po dokončení tohoto článku, budete mít vlastní zásady registrace a přihlášení pomocí značky a vzhled. S Azure Active Directory B2C (Azure AD B2C), můžete získat téměř plnou kontrolu nad hello obsah HTML a CSS, má uvedené toousers. Pokud používáte vlastní zásadu, nakonfigurujete přizpůsobení uživatelského rozhraní v kódu XML místo použití ovládacích prvků v hello portálu Azure. 

## <a name="prerequisites"></a>Požadavky

Než začnete, dokončení [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md). Měli byste mít pracovní vlastní zásadu pro registraci a přihlašování s místní účty.

## <a name="page-ui-customization"></a>Přizpůsobení uživatelského rozhraní stránky

Pomocí funkce přizpůsobení uživatelského rozhraní stránky hello můžete přizpůsobit hello vzhledu a chování žádné vlastní zásady. Můžete také udržovat značky a visual konzistenci mezi aplikací a Azure AD B2C.

Zde je, jak to funguje: Azure AD B2C spuštěním kódu v prohlížeči vašeho zákazníka a používá moderní přístup názvem [sdílení prostředků různých původů (CORS)](http://www.w3.org/TR/cors/). Nejdřív zadejte adresu URL ve vlastních zásadách hello s přizpůsobený obsah HTML. Azure AD B2C sloučí elementy uživatelského rozhraní pomocí hello obsah HTML, který je načten z vaší adresy URL a potom zobrazí hello stránky toohello zákazníka.

## <a name="create-your-html5-content"></a>Vytvoření vaší HTML5 obsahu

Vytváření obsahu s názvem vašeho produktu značky HTML v hlavě hello.

1. Zkopírujte hello následující fragment kódu HTML. Je ve správném formátu názvem HTML5 s prázdný element  *\<div id = "api"\>\</div\>*  umístěné v rámci hello  *\<textu\>*  značky. Tento element určuje, kde je obsah Azure AD B2C toobe vložit.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

   >[!NOTE]
   >Z bezpečnostních důvodů je aktuálně blokován hello pomocí jazyka JavaScript pro přizpůsobení.

2. Vložte fragment hello zkopírovali v textovém editoru a pak uložte soubor hello jako *přizpůsobit ui.html*.

## <a name="create-an-azure-blob-storage-account"></a>Vytvoření účtu úložiště objektů Blob v Azure

>[!NOTE]
> V tomto článku používáme toohost úložiště objektů Blob v Azure náš obsah. Můžete zvolit toohost obsah na webovém serveru, ale musíte [povolení CORS na vašem webovém serveru](https://enable-cors.org/server.html).

toohost tento obsah HTML v úložišti objektů Blob, hello následující:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na hello **rozbočovače** nabídce vyberte možnost **nový** > **úložiště** > **účet úložiště**.
3. Zadejte jedinečný **název** pro váš účet úložiště.
4. **Model nasazení** může zůstat **Resource Manager**.
5. Změna **druh účtu** příliš**úložiště objektů Blob**.
6. **Výkon** může zůstat **standardní**.
7. **Replikace** může zůstat **RA-GRS**.
8. **Úroveň přístupu** může zůstat **horká**.
9. **Šifrování služby úložiště** může zůstat **zakázané**.
10. Vyberte **předplatné** pro váš účet úložiště.
11. Vytvoření **skupiny prostředků** nebo vyberte nějaký existující.
12. Vyberte hello **zeměpisné polohy** pro váš účet úložiště.
13. Klikněte na tlačítko **vytvořit** účet úložiště toocreate hello.  
    Po dokončení nasazení hello hello **účet úložiště** automaticky otevře se okno.

## <a name="create-a-container"></a>Vytvoření kontejneru

toocreate veřejném kontejneru v úložiště objektů Blob, hello následující:

1. Klikněte na tlačítko hello **přehled** kartě.
2. Klikněte na tlačítko **kontejneru**.
3. Pro **název**, typ **$root**.
4. Nastavit **přistupovat typu** příliš**Blob**.
5. Klikněte na tlačítko **$root** tooopen hello nový kontejner.
6. Klikněte na **Odeslat**.
7. Klikněte na ikonu složky hello další příliš**vyberte soubor**.
8. Přejděte příliš**přizpůsobit ui.html**, který jste vytvořili dříve v hello [přizpůsobení uživatelského rozhraní stránky](#the-page-ui-customization-feature) části.
9. Klikněte na **Odeslat**.
10. Vyberte hello přizpůsobit ui.html objekt blob, který jste nahráli.
11. Další příliš**URL**, klikněte na tlačítko **kopie**.
12. V prohlížeči vložte adresu URL zkopírovat hello a toohello společnosti. Pokud lokalita hello je nedostupné, ujistěte se, typ přístupu kontejneru hello je nastaven příliš**blob**.

## <a name="configure-cors"></a>Konfigurace CORS

Nakonfigurujte úložiště objektů Blob pro sdílení prostředků různého původu pomocí hello následující:

>[!NOTE]
>Chcete tootry out hello funkce přizpůsobení uživatelského rozhraní pomocí našich ukázkový kód HTML a CSS obsah? Nabízíme [jednoduché pomocný nástroj](active-directory-b2c-reference-ui-customization-helper-tool.md) , odešle a nakonfiguruje naše ukázkový obsah na vašem účtu úložiště objektů Blob. Pokud použijete nástroj hello, přeskočit příliš[upravte registrace nebo přihlášení vlastní zásady](#modify-your-sign-up-or-sign-in-custom-policy).

1. Na hello **úložiště** okno, v části **nastavení**, otevřete **CORS**.
2. Klikněte na tlačítko **Přidat**.
3. Pro **povolené zdroje**, zadejte hvězdičku (\*).
4. V hello **povolených operací** rozevíracího seznamu, vyberte **získat** a **možnosti**.
5. Pro **povolené hlavičky**, zadejte hvězdičku (\*).
6. Pro **zveřejněné hlavičky**, zadejte hvězdičku (\*).
7. Pro **maximální stáří (v sekundách)**, typ **200**.
8. Klikněte na tlačítko **Přidat**.

## <a name="test-cors"></a>Test CORS

Ověřte, že jste připraveni díky hello následující:

1. Přejděte toohello [test cors.org](http://test-cors.org/) webu a vložte adresu URL hello v hello **vzdálenou adresou URL** pole.
2. Klikněte na tlačítko **poslat žádost o**.  
    Pokud se zobrazí chyba, ujistěte se, že vaše [nastavení CORS](#configure-cors) jsou správné. Může také potřebovat tooclear vaší mezipaměti prohlížeče nebo otevřít relaci procházení v privátní stisknutím kombinace kláves Ctrl + Shift + P.

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a>Upravit vlastní zásady registrace nebo přihlášení

V části hello nejvyšší úrovně  *\<TrustFrameworkPolicy\>*  značka, byste měli najít  *\<BuildingBlocks\>*  značky. V rámci hello  *\<BuildingBlocks\>*  přidat značky,  *\<ContentDefinitions\>*  značky zkopírováním hello následující ukázka. Nahraďte *your_storage_account* hello název účtu úložiště.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a>Nahrát váš aktualizované vlastní zásady

1. V hello [portál Azure](https://portal.azure.com), [přepnout do kontextu hello klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a pak otevřete hello **Azure AD B2C** okno.
2. Klikněte na tlačítko **všechny zásady**.
3. Klikněte na tlačítko **nahrát zásady**.
4. Nahrát `SignUpOrSignin.xml` s hello  *\<ContentDefinitions\>*  značky, které jste přidali dříve.

## <a name="test-hello-custom-policy-by-using-run-now"></a>Otestovat pomocí vlastních zásad pro hello **spustit nyní**

1. Na hello **Azure AD B2C** okně přejděte příliš**všechny zásady**.
2. Vyberte hello vlastní zásadu, kterou jste nahráli a klikněte na tlačítko hello **spustit nyní** tlačítko.
3. Až byste měli mít toosign pomocí e-mailovou adresu.

## <a name="reference"></a>Referenční informace

Pro přizpůsobení uživatelského rozhraní Zde můžete najít ukázkové šablony:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Hello sample_templates/wingtip složka obsahuje následující soubory HTML hello:

| HTML5 šablony | Popis |
|----------------|-------------|
| *phonefactor.HTML* | Tento soubor jako šablonu použijte pro stránku služby Multi-Factor authentication. |
| *ResetPassword.HTML* | Použít jako šablonu pro tento soubor zapomněli jste heslo?. |
| *selfasserted.HTML* | Tento soubor jako šablonu použijte pro stránku pro přihlášení sociálních účet, stránku pro přihlášení místní účet nebo na přihlašovací stránku místní účet. |
| *Unified.HTML* | Tento soubor jako šablonu použijte pro jednotné stránku registrace nebo přihlášení. |
| *updateprofile.HTML* | Tento soubor jako šablonu použijte pro stránku aktualizace profilu. |

V hello [upravte část vaší vlastní zásady registrace nebo přihlášení](#modify-your-sign-up-or-sign-in-custom-policy), jste nakonfigurovali hello obsahu definice `api.idpselections`. úplnou sadu Hello obsahu definice ID, které jsou rozpoznány identity prostředí hello Azure AD B2C a jejich popisy jsou v následující tabulce hello:

| ID obsahu definice | Popis | 
|-----------------------|-------------|
| *API.Error* | **Chybové stránky**. Tato stránka se zobrazí, když je došlo k výjimce nebo došlo k chybě. |
| *API.idpselections* | **Stránka Výběr zprostředkovatele identity**. Tato stránka obsahuje seznam identity, které můžete zvolit zprostředkovatelé, kteří hello uživatele při přihlášení. Tyto možnosti jsou poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google + nebo místním účtům. |
| *API.idpselections.Signup* | **Výběr zprostředkovatele identity pro registraci**. Tato stránka obsahuje seznam identity, které můžete zvolit zprostředkovatelé, kteří hello uživatele během registrace. Tyto možnosti jsou poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google + nebo místním účtům. |
| *API.localaccountpasswordreset* | **Zapomněli jste heslo**. Tato stránka obsahuje formuláře tento uživatel hello musíte dokončit tooinitiate pro resetování hesla.  |
| *API.localaccountsignin* | **Přihlašovací stránka místní účet**. Tato stránka obsahuje formulář přihlášení pro přihlášení pomocí místního účtu, který je založený na e-mailovou adresu nebo uživatelské jméno. Hello formulář může obsahovat vstupní textové pole a pole pro zadání hesla. |
| *API.localaccountsignup* | **Místní účet stránku**. Tato stránka obsahuje registrační formulář pro registraci pro místní účet, který je založený na e-mailovou adresu nebo uživatelské jméno. Hello formulář může obsahovat různé vstupní ovládací prvky, jako je například vstupní textové pole, pole pro zadání hesla, přepínače, pole rozevíracího seznamu vyberte jeden a vybrat víc zaškrtávací políčka. |
| *API.phonefactor* | **Stránka služby Multi-Factor authentication**. Na této stránce uživatelé mohli ověřit jejich telefonní čísla (pomocí textové nebo hlasové) během registrace nebo přihlášení. |
| *API.selfasserted* | **Stránku pro přihlášení sociálních účet**. Tato stránka obsahuje registrační formulář, který uživatelé musí dokončit při registraci pomocí stávající účet od poskytovatele identity sociálních třeba Facebook nebo Google +. Tato stránka je podobné toohello předcházející sociálních účet stránku pro přihlášení, s výjimkou pole pro zadání hesla hello. |
| *API.selfasserted.profileupdate* | **Stránka pro aktualizaci profilu**. Tato stránka obsahuje formuláře, které uživatelé mohou používat tooupdate svůj profil. Tato stránka je podobné toohello sociálních registrační stránku účtu, s výjimkou pole pro zadání hesla hello. |
| *API.signuporsignin* | **Jednotná stránku registrace nebo přihlášení**. Tato stránka zpracuje obě hello registrace a přihlašování uživatelů, kteří můžou využívat poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook nebo Google + nebo místní účty.  |

## <a name="next-steps"></a>Další kroky

Další informace o prvky uživatelského rozhraní, které se dají přizpůsobit najdete v tématu [referenční příručka pro přizpůsobení uživatelského rozhraní pro předdefinované zásady](active-directory-b2c-reference-ui-customization.md).
