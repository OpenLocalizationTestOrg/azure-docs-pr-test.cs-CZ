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
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a><span data-ttu-id="76b70-103">Azure Active Directory B2C: Konfigurace ve vlastních zásadách pro přizpůsobení uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="76b70-103">Azure Active Directory B2C: Configure UI customization in a custom policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="76b70-104">Po dokončení tohoto článku, budete mít vlastní zásady registrace a přihlášení pomocí značky a vzhled.</span><span class="sxs-lookup"><span data-stu-id="76b70-104">After you complete this article, you will have a sign-up and sign-in custom policy with your brand and appearance.</span></span> <span data-ttu-id="76b70-105">S Azure Active Directory B2C (Azure AD B2C), můžete získat téměř plnou kontrolu nad hello obsah HTML a CSS, má uvedené toousers.</span><span class="sxs-lookup"><span data-stu-id="76b70-105">With Azure Active Directory B2C (Azure AD B2C), you get nearly full control of hello HTML and CSS content that's presented toousers.</span></span> <span data-ttu-id="76b70-106">Pokud používáte vlastní zásadu, nakonfigurujete přizpůsobení uživatelského rozhraní v kódu XML místo použití ovládacích prvků v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="76b70-106">When you use a custom policy, you configure UI customization in XML instead of using controls in hello Azure portal.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="76b70-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="76b70-107">Prerequisites</span></span>

<span data-ttu-id="76b70-108">Než začnete, dokončení [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="76b70-108">Before you begin, complete [Getting started with custom policies](active-directory-b2c-get-started-custom.md).</span></span> <span data-ttu-id="76b70-109">Měli byste mít pracovní vlastní zásadu pro registraci a přihlašování s místní účty.</span><span class="sxs-lookup"><span data-stu-id="76b70-109">You should have a working custom policy for sign-up and sign-in with local accounts.</span></span>

## <a name="page-ui-customization"></a><span data-ttu-id="76b70-110">Přizpůsobení uživatelského rozhraní stránky</span><span class="sxs-lookup"><span data-stu-id="76b70-110">Page UI customization</span></span>

<span data-ttu-id="76b70-111">Pomocí funkce přizpůsobení uživatelského rozhraní stránky hello můžete přizpůsobit hello vzhledu a chování žádné vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="76b70-111">By using hello page UI customization feature, you can customize hello look and feel of any custom policy.</span></span> <span data-ttu-id="76b70-112">Můžete také udržovat značky a visual konzistenci mezi aplikací a Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="76b70-112">You can also maintain brand and visual consistency between your application and Azure AD B2C.</span></span>

<span data-ttu-id="76b70-113">Zde je, jak to funguje: Azure AD B2C spuštěním kódu v prohlížeči vašeho zákazníka a používá moderní přístup názvem [sdílení prostředků různých původů (CORS)](http://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="76b70-113">Here's how it works: Azure AD B2C runs code in your customer's browser and uses a modern approach called [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span></span> <span data-ttu-id="76b70-114">Nejdřív zadejte adresu URL ve vlastních zásadách hello s přizpůsobený obsah HTML.</span><span class="sxs-lookup"><span data-stu-id="76b70-114">First, you specify a URL in hello custom policy with customized HTML content.</span></span> <span data-ttu-id="76b70-115">Azure AD B2C sloučí elementy uživatelského rozhraní pomocí hello obsah HTML, který je načten z vaší adresy URL a potom zobrazí hello stránky toohello zákazníka.</span><span class="sxs-lookup"><span data-stu-id="76b70-115">Azure AD B2C merges UI elements with hello HTML content that's loaded from your URL and then displays hello page toohello customer.</span></span>

## <a name="create-your-html5-content"></a><span data-ttu-id="76b70-116">Vytvoření vaší HTML5 obsahu</span><span class="sxs-lookup"><span data-stu-id="76b70-116">Create your HTML5 content</span></span>

<span data-ttu-id="76b70-117">Vytváření obsahu s názvem vašeho produktu značky HTML v hlavě hello.</span><span class="sxs-lookup"><span data-stu-id="76b70-117">Create HTML content with your product's brand name in hello title.</span></span>

1. <span data-ttu-id="76b70-118">Zkopírujte hello následující fragment kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="76b70-118">Copy hello following HTML snippet.</span></span> <span data-ttu-id="76b70-119">Je ve správném formátu názvem HTML5 s prázdný element  *\<div id = "api"\>\</div\>*  umístěné v rámci hello  *\<textu\>*  značky.</span><span class="sxs-lookup"><span data-stu-id="76b70-119">It is well-formed HTML5 with an empty element called *\<div id="api"\>\</div\>* located within hello *\<body\>* tags.</span></span> <span data-ttu-id="76b70-120">Tento element určuje, kde je obsah Azure AD B2C toobe vložit.</span><span class="sxs-lookup"><span data-stu-id="76b70-120">This element indicates where Azure AD B2C content is toobe inserted.</span></span>

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
   ><span data-ttu-id="76b70-121">Z bezpečnostních důvodů je aktuálně blokován hello pomocí jazyka JavaScript pro přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="76b70-121">For security reasons, hello use of JavaScript is currently blocked for customization.</span></span>

2. <span data-ttu-id="76b70-122">Vložte fragment hello zkopírovali v textovém editoru a pak uložte soubor hello jako *přizpůsobit ui.html*.</span><span class="sxs-lookup"><span data-stu-id="76b70-122">Paste hello copied snippet in a text editor, and then save hello file as *customize-ui.html*.</span></span>

## <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="76b70-123">Vytvoření účtu úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="76b70-123">Create an Azure Blob storage account</span></span>

>[!NOTE]
> <span data-ttu-id="76b70-124">V tomto článku používáme toohost úložiště objektů Blob v Azure náš obsah.</span><span class="sxs-lookup"><span data-stu-id="76b70-124">In this article, we use Azure Blob storage toohost our content.</span></span> <span data-ttu-id="76b70-125">Můžete zvolit toohost obsah na webovém serveru, ale musíte [povolení CORS na vašem webovém serveru](https://enable-cors.org/server.html).</span><span class="sxs-lookup"><span data-stu-id="76b70-125">You can choose toohost your content on a web server, but you must [enable CORS on your web server](https://enable-cors.org/server.html).</span></span>

<span data-ttu-id="76b70-126">toohost tento obsah HTML v úložišti objektů Blob, hello následující:</span><span class="sxs-lookup"><span data-stu-id="76b70-126">toohost this HTML content in Blob storage, do hello following:</span></span>

1. <span data-ttu-id="76b70-127">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="76b70-127">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="76b70-128">Na hello **rozbočovače** nabídce vyberte možnost **nový** > **úložiště** > **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="76b70-128">On hello **Hub** menu, select **New** > **Storage** > **Storage account**.</span></span>
3. <span data-ttu-id="76b70-129">Zadejte jedinečný **název** pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="76b70-129">Enter a unique **Name** for your storage account.</span></span>
4. <span data-ttu-id="76b70-130">**Model nasazení** může zůstat **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="76b70-130">**Deployment model** can remain **Resource Manager**.</span></span>
5. <span data-ttu-id="76b70-131">Změna **druh účtu** příliš**úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="76b70-131">Change **Account Kind** too**Blob storage**.</span></span>
6. <span data-ttu-id="76b70-132">**Výkon** může zůstat **standardní**.</span><span class="sxs-lookup"><span data-stu-id="76b70-132">**Performance** can remain **Standard**.</span></span>
7. <span data-ttu-id="76b70-133">**Replikace** může zůstat **RA-GRS**.</span><span class="sxs-lookup"><span data-stu-id="76b70-133">**Replication** can remain **RA-GRS**.</span></span>
8. <span data-ttu-id="76b70-134">**Úroveň přístupu** může zůstat **horká**.</span><span class="sxs-lookup"><span data-stu-id="76b70-134">**Access tier** can remain **Hot**.</span></span>
9. <span data-ttu-id="76b70-135">**Šifrování služby úložiště** může zůstat **zakázané**.</span><span class="sxs-lookup"><span data-stu-id="76b70-135">**Storage service encryption** can remain **Disabled**.</span></span>
10. <span data-ttu-id="76b70-136">Vyberte **předplatné** pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="76b70-136">Select a **Subscription** for your storage account.</span></span>
11. <span data-ttu-id="76b70-137">Vytvoření **skupiny prostředků** nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="76b70-137">Create a **Resource group** or select an existing one.</span></span>
12. <span data-ttu-id="76b70-138">Vyberte hello **zeměpisné polohy** pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="76b70-138">Select hello **Geographic location** for your storage account.</span></span>
13. <span data-ttu-id="76b70-139">Klikněte na tlačítko **vytvořit** účet úložiště toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="76b70-139">Click **Create** toocreate hello storage account.</span></span>  
    <span data-ttu-id="76b70-140">Po dokončení nasazení hello hello **účet úložiště** automaticky otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="76b70-140">After hello deployment is completed, hello **Storage account** blade opens automatically.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="76b70-141">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="76b70-141">Create a container</span></span>

<span data-ttu-id="76b70-142">toocreate veřejném kontejneru v úložiště objektů Blob, hello následující:</span><span class="sxs-lookup"><span data-stu-id="76b70-142">toocreate a public container in Blob storage, do hello following:</span></span>

1. <span data-ttu-id="76b70-143">Klikněte na tlačítko hello **přehled** kartě.</span><span class="sxs-lookup"><span data-stu-id="76b70-143">Click hello **Overview** tab.</span></span>
2. <span data-ttu-id="76b70-144">Klikněte na tlačítko **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="76b70-144">Click **Container**.</span></span>
3. <span data-ttu-id="76b70-145">Pro **název**, typ **$root**.</span><span class="sxs-lookup"><span data-stu-id="76b70-145">For **Name**, type **$root**.</span></span>
4. <span data-ttu-id="76b70-146">Nastavit **přistupovat typu** příliš**Blob**.</span><span class="sxs-lookup"><span data-stu-id="76b70-146">Set **Access type** too**Blob**.</span></span>
5. <span data-ttu-id="76b70-147">Klikněte na tlačítko **$root** tooopen hello nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="76b70-147">Click **$root** tooopen hello new container.</span></span>
6. <span data-ttu-id="76b70-148">Klikněte na **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="76b70-148">Click **Upload**.</span></span>
7. <span data-ttu-id="76b70-149">Klikněte na ikonu složky hello další příliš**vyberte soubor**.</span><span class="sxs-lookup"><span data-stu-id="76b70-149">Click hello folder icon next too**Select a file**.</span></span>
8. <span data-ttu-id="76b70-150">Přejděte příliš**přizpůsobit ui.html**, který jste vytvořili dříve v hello [přizpůsobení uživatelského rozhraní stránky](#the-page-ui-customization-feature) části.</span><span class="sxs-lookup"><span data-stu-id="76b70-150">Go too**customize-ui.html**, which you created earlier in hello [Page UI customization](#the-page-ui-customization-feature) section.</span></span>
9. <span data-ttu-id="76b70-151">Klikněte na **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="76b70-151">Click **Upload**.</span></span>
10. <span data-ttu-id="76b70-152">Vyberte hello přizpůsobit ui.html objekt blob, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="76b70-152">Select hello customize-ui.html blob that you uploaded.</span></span>
11. <span data-ttu-id="76b70-153">Další příliš**URL**, klikněte na tlačítko **kopie**.</span><span class="sxs-lookup"><span data-stu-id="76b70-153">Next too**URL**, click **Copy**.</span></span>
12. <span data-ttu-id="76b70-154">V prohlížeči vložte adresu URL zkopírovat hello a toohello společnosti.</span><span class="sxs-lookup"><span data-stu-id="76b70-154">In a browser, paste hello copied URL, and go toohello site.</span></span> <span data-ttu-id="76b70-155">Pokud lokalita hello je nedostupné, ujistěte se, typ přístupu kontejneru hello je nastaven příliš**blob**.</span><span class="sxs-lookup"><span data-stu-id="76b70-155">If hello site is inaccessible, make sure hello container access type is set too**blob**.</span></span>

## <a name="configure-cors"></a><span data-ttu-id="76b70-156">Konfigurace CORS</span><span class="sxs-lookup"><span data-stu-id="76b70-156">Configure CORS</span></span>

<span data-ttu-id="76b70-157">Nakonfigurujte úložiště objektů Blob pro sdílení prostředků různého původu pomocí hello následující:</span><span class="sxs-lookup"><span data-stu-id="76b70-157">Configure Blob storage for Cross-Origin Resource Sharing by doing hello following:</span></span>

>[!NOTE]
><span data-ttu-id="76b70-158">Chcete tootry out hello funkce přizpůsobení uživatelského rozhraní pomocí našich ukázkový kód HTML a CSS obsah?</span><span class="sxs-lookup"><span data-stu-id="76b70-158">Want tootry out hello UI customization feature by using our sample HTML and CSS content?</span></span> <span data-ttu-id="76b70-159">Nabízíme [jednoduché pomocný nástroj](active-directory-b2c-reference-ui-customization-helper-tool.md) , odešle a nakonfiguruje naše ukázkový obsah na vašem účtu úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="76b70-159">We've provided [a simple helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) that uploads and configures our sample content on your Blob storage account.</span></span> <span data-ttu-id="76b70-160">Pokud použijete nástroj hello, přeskočit příliš[upravte registrace nebo přihlášení vlastní zásady](#modify-your-sign-up-or-sign-in-custom-policy).</span><span class="sxs-lookup"><span data-stu-id="76b70-160">If you use hello tool, skip ahead too[Modify your sign-up or sign-in custom policy](#modify-your-sign-up-or-sign-in-custom-policy).</span></span>

1. <span data-ttu-id="76b70-161">Na hello **úložiště** okno, v části **nastavení**, otevřete **CORS**.</span><span class="sxs-lookup"><span data-stu-id="76b70-161">On hello **Storage** blade, under **Settings**, open **CORS**.</span></span>
2. <span data-ttu-id="76b70-162">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="76b70-162">Click **Add**.</span></span>
3. <span data-ttu-id="76b70-163">Pro **povolené zdroje**, zadejte hvězdičku (\*).</span><span class="sxs-lookup"><span data-stu-id="76b70-163">For **Allowed origins**, type an asterisk (\*).</span></span>
4. <span data-ttu-id="76b70-164">V hello **povolených operací** rozevíracího seznamu, vyberte **získat** a **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="76b70-164">In hello **Allowed verbs** drop-down list, select both **GET** and **OPTIONS**.</span></span>
5. <span data-ttu-id="76b70-165">Pro **povolené hlavičky**, zadejte hvězdičku (\*).</span><span class="sxs-lookup"><span data-stu-id="76b70-165">For **Allowed headers**, type an asterisk (\*).</span></span>
6. <span data-ttu-id="76b70-166">Pro **zveřejněné hlavičky**, zadejte hvězdičku (\*).</span><span class="sxs-lookup"><span data-stu-id="76b70-166">For **Exposed headers**, type an asterisk (\*).</span></span>
7. <span data-ttu-id="76b70-167">Pro **maximální stáří (v sekundách)**, typ **200**.</span><span class="sxs-lookup"><span data-stu-id="76b70-167">For **Maximum age (seconds)**, type **200**.</span></span>
8. <span data-ttu-id="76b70-168">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="76b70-168">Click **Add**.</span></span>

## <a name="test-cors"></a><span data-ttu-id="76b70-169">Test CORS</span><span class="sxs-lookup"><span data-stu-id="76b70-169">Test CORS</span></span>

<span data-ttu-id="76b70-170">Ověřte, že jste připraveni díky hello následující:</span><span class="sxs-lookup"><span data-stu-id="76b70-170">Validate that you're ready by doing hello following:</span></span>

1. <span data-ttu-id="76b70-171">Přejděte toohello [test cors.org](http://test-cors.org/) webu a vložte adresu URL hello v hello **vzdálenou adresou URL** pole.</span><span class="sxs-lookup"><span data-stu-id="76b70-171">Go toohello [test-cors.org](http://test-cors.org/) website, and then paste hello URL in hello **Remote URL** box.</span></span>
2. <span data-ttu-id="76b70-172">Klikněte na tlačítko **poslat žádost o**.</span><span class="sxs-lookup"><span data-stu-id="76b70-172">Click **Send Request**.</span></span>  
    <span data-ttu-id="76b70-173">Pokud se zobrazí chyba, ujistěte se, že vaše [nastavení CORS](#configure-cors) jsou správné.</span><span class="sxs-lookup"><span data-stu-id="76b70-173">If you receive an error, make sure that your [CORS settings](#configure-cors) are correct.</span></span> <span data-ttu-id="76b70-174">Může také potřebovat tooclear vaší mezipaměti prohlížeče nebo otevřít relaci procházení v privátní stisknutím kombinace kláves Ctrl + Shift + P.</span><span class="sxs-lookup"><span data-stu-id="76b70-174">You might also need tooclear your browser cache or open an in-private browsing session by pressing Ctrl+Shift+P.</span></span>

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a><span data-ttu-id="76b70-175">Upravit vlastní zásady registrace nebo přihlášení</span><span class="sxs-lookup"><span data-stu-id="76b70-175">Modify your sign-up or sign-in custom policy</span></span>

<span data-ttu-id="76b70-176">V části hello nejvyšší úrovně  *\<TrustFrameworkPolicy\>*  značka, byste měli najít  *\<BuildingBlocks\>*  značky.</span><span class="sxs-lookup"><span data-stu-id="76b70-176">Under hello top-level *\<TrustFrameworkPolicy\>* tag, you should find *\<BuildingBlocks\>* tag.</span></span> <span data-ttu-id="76b70-177">V rámci hello  *\<BuildingBlocks\>*  přidat značky,  *\<ContentDefinitions\>*  značky zkopírováním hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="76b70-177">Within hello *\<BuildingBlocks\>* tags, add a *\<ContentDefinitions\>* tag by copying hello following example.</span></span> <span data-ttu-id="76b70-178">Nahraďte *your_storage_account* hello název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="76b70-178">Replace *your_storage_account* with hello name of your storage account.</span></span>

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a><span data-ttu-id="76b70-179">Nahrát váš aktualizované vlastní zásady</span><span class="sxs-lookup"><span data-stu-id="76b70-179">Upload your updated custom policy</span></span>

1. <span data-ttu-id="76b70-180">V hello [portál Azure](https://portal.azure.com), [přepnout do kontextu hello klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a pak otevřete hello **Azure AD B2C** okno.</span><span class="sxs-lookup"><span data-stu-id="76b70-180">In hello [Azure portal](https://portal.azure.com), [switch into hello context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then open hello **Azure AD B2C** blade.</span></span>
2. <span data-ttu-id="76b70-181">Klikněte na tlačítko **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="76b70-181">Click **All Policies**.</span></span>
3. <span data-ttu-id="76b70-182">Klikněte na tlačítko **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="76b70-182">Click **Upload Policy**.</span></span>
4. <span data-ttu-id="76b70-183">Nahrát `SignUpOrSignin.xml` s hello  *\<ContentDefinitions\>*  značky, které jste přidali dříve.</span><span class="sxs-lookup"><span data-stu-id="76b70-183">Upload `SignUpOrSignin.xml` with hello *\<ContentDefinitions\>* tag that you added previously.</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="76b70-184">Otestovat pomocí vlastních zásad pro hello **spustit nyní**</span><span class="sxs-lookup"><span data-stu-id="76b70-184">Test hello custom policy by using **Run now**</span></span>

1. <span data-ttu-id="76b70-185">Na hello **Azure AD B2C** okně přejděte příliš**všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="76b70-185">On hello **Azure AD B2C** blade, go too**All polices**.</span></span>
2. <span data-ttu-id="76b70-186">Vyberte hello vlastní zásadu, kterou jste nahráli a klikněte na tlačítko hello **spustit nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="76b70-186">Select hello custom policy that you uploaded, and click hello **Run now** button.</span></span>
3. <span data-ttu-id="76b70-187">Až byste měli mít toosign pomocí e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="76b70-187">You should be able toosign up by using an email address.</span></span>

## <a name="reference"></a><span data-ttu-id="76b70-188">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="76b70-188">Reference</span></span>

<span data-ttu-id="76b70-189">Pro přizpůsobení uživatelského rozhraní Zde můžete najít ukázkové šablony:</span><span class="sxs-lookup"><span data-stu-id="76b70-189">You can find sample templates for UI customization here:</span></span>

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

<span data-ttu-id="76b70-190">Hello sample_templates/wingtip složka obsahuje následující soubory HTML hello:</span><span class="sxs-lookup"><span data-stu-id="76b70-190">hello sample_templates/wingtip folder contains hello following HTML files:</span></span>

| <span data-ttu-id="76b70-191">HTML5 šablony</span><span class="sxs-lookup"><span data-stu-id="76b70-191">HTML5 template</span></span> | <span data-ttu-id="76b70-192">Popis</span><span class="sxs-lookup"><span data-stu-id="76b70-192">Description</span></span> |
|----------------|-------------|
| <span data-ttu-id="76b70-193">*phonefactor.HTML*</span><span class="sxs-lookup"><span data-stu-id="76b70-193">*phonefactor.html*</span></span> | <span data-ttu-id="76b70-194">Tento soubor jako šablonu použijte pro stránku služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="76b70-194">Use this file as a template for a multi-factor authentication page.</span></span> |
| <span data-ttu-id="76b70-195">*ResetPassword.HTML*</span><span class="sxs-lookup"><span data-stu-id="76b70-195">*resetpassword.html*</span></span> | <span data-ttu-id="76b70-196">Použít jako šablonu pro tento soubor zapomněli jste heslo?.</span><span class="sxs-lookup"><span data-stu-id="76b70-196">Use this file as a template for a forgot password page.</span></span> |
| <span data-ttu-id="76b70-197">*selfasserted.HTML*</span><span class="sxs-lookup"><span data-stu-id="76b70-197">*selfasserted.html*</span></span> | <span data-ttu-id="76b70-198">Tento soubor jako šablonu použijte pro stránku pro přihlášení sociálních účet, stránku pro přihlášení místní účet nebo na přihlašovací stránku místní účet.</span><span class="sxs-lookup"><span data-stu-id="76b70-198">Use this file as a template for a social account sign-up page, a local account sign-up page, or a local account sign-in page.</span></span> |
| <span data-ttu-id="76b70-199">*Unified.HTML*</span><span class="sxs-lookup"><span data-stu-id="76b70-199">*unified.html*</span></span> | <span data-ttu-id="76b70-200">Tento soubor jako šablonu použijte pro jednotné stránku registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="76b70-200">Use this file as a template for a unified sign-up or sign-in page.</span></span> |
| <span data-ttu-id="76b70-201">*updateprofile.HTML*</span><span class="sxs-lookup"><span data-stu-id="76b70-201">*updateprofile.html*</span></span> | <span data-ttu-id="76b70-202">Tento soubor jako šablonu použijte pro stránku aktualizace profilu.</span><span class="sxs-lookup"><span data-stu-id="76b70-202">Use this file as a template for a profile update page.</span></span> |

<span data-ttu-id="76b70-203">V hello [upravte část vaší vlastní zásady registrace nebo přihlášení](#modify-your-sign-up-or-sign-in-custom-policy), jste nakonfigurovali hello obsahu definice `api.idpselections`.</span><span class="sxs-lookup"><span data-stu-id="76b70-203">In hello [Modify your sign-up or sign-in custom policy section](#modify-your-sign-up-or-sign-in-custom-policy), you configured hello content definition for `api.idpselections`.</span></span> <span data-ttu-id="76b70-204">úplnou sadu Hello obsahu definice ID, které jsou rozpoznány identity prostředí hello Azure AD B2C a jejich popisy jsou v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="76b70-204">hello full set of content definition IDs that are recognized by hello Azure AD B2C identity experience framework and their descriptions are in hello following table:</span></span>

| <span data-ttu-id="76b70-205">ID obsahu definice</span><span class="sxs-lookup"><span data-stu-id="76b70-205">Content definition ID</span></span> | <span data-ttu-id="76b70-206">Popis</span><span class="sxs-lookup"><span data-stu-id="76b70-206">Description</span></span> | 
|-----------------------|-------------|
| <span data-ttu-id="76b70-207">*API.Error*</span><span class="sxs-lookup"><span data-stu-id="76b70-207">*api.error*</span></span> | <span data-ttu-id="76b70-208">**Chybové stránky**.</span><span class="sxs-lookup"><span data-stu-id="76b70-208">**Error page**.</span></span> <span data-ttu-id="76b70-209">Tato stránka se zobrazí, když je došlo k výjimce nebo došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="76b70-209">This page is displayed when an exception or an error is encountered.</span></span> |
| <span data-ttu-id="76b70-210">*API.idpselections*</span><span class="sxs-lookup"><span data-stu-id="76b70-210">*api.idpselections*</span></span> | <span data-ttu-id="76b70-211">**Stránka Výběr zprostředkovatele identity**.</span><span class="sxs-lookup"><span data-stu-id="76b70-211">**Identity provider selection page**.</span></span> <span data-ttu-id="76b70-212">Tato stránka obsahuje seznam identity, které můžete zvolit zprostředkovatelé, kteří hello uživatele při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="76b70-212">This page contains a list of identity providers that hello user can choose from during sign-in.</span></span> <span data-ttu-id="76b70-213">Tyto možnosti jsou poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google + nebo místním účtům.</span><span class="sxs-lookup"><span data-stu-id="76b70-213">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="76b70-214">*API.idpselections.Signup*</span><span class="sxs-lookup"><span data-stu-id="76b70-214">*api.idpselections.signup*</span></span> | <span data-ttu-id="76b70-215">**Výběr zprostředkovatele identity pro registraci**.</span><span class="sxs-lookup"><span data-stu-id="76b70-215">**Identity provider selection for sign-up**.</span></span> <span data-ttu-id="76b70-216">Tato stránka obsahuje seznam identity, které můžete zvolit zprostředkovatelé, kteří hello uživatele během registrace.</span><span class="sxs-lookup"><span data-stu-id="76b70-216">This page contains a list of identity providers that hello user can choose from during sign-up.</span></span> <span data-ttu-id="76b70-217">Tyto možnosti jsou poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google + nebo místním účtům.</span><span class="sxs-lookup"><span data-stu-id="76b70-217">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="76b70-218">*API.localaccountpasswordreset*</span><span class="sxs-lookup"><span data-stu-id="76b70-218">*api.localaccountpasswordreset*</span></span> | <span data-ttu-id="76b70-219">**Zapomněli jste heslo**.</span><span class="sxs-lookup"><span data-stu-id="76b70-219">**Forgot password page**.</span></span> <span data-ttu-id="76b70-220">Tato stránka obsahuje formuláře tento uživatel hello musíte dokončit tooinitiate pro resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="76b70-220">This page contains a form that hello user must complete tooinitiate a password reset.</span></span>  |
| <span data-ttu-id="76b70-221">*API.localaccountsignin*</span><span class="sxs-lookup"><span data-stu-id="76b70-221">*api.localaccountsignin*</span></span> | <span data-ttu-id="76b70-222">**Přihlašovací stránka místní účet**.</span><span class="sxs-lookup"><span data-stu-id="76b70-222">**Local account sign-in page**.</span></span> <span data-ttu-id="76b70-223">Tato stránka obsahuje formulář přihlášení pro přihlášení pomocí místního účtu, který je založený na e-mailovou adresu nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="76b70-223">This page contains a sign-in form for signing in with a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="76b70-224">Hello formulář může obsahovat vstupní textové pole a pole pro zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="76b70-224">hello form can contain a text input box and password entry box.</span></span> |
| <span data-ttu-id="76b70-225">*API.localaccountsignup*</span><span class="sxs-lookup"><span data-stu-id="76b70-225">*api.localaccountsignup*</span></span> | <span data-ttu-id="76b70-226">**Místní účet stránku**.</span><span class="sxs-lookup"><span data-stu-id="76b70-226">**Local account sign-up page**.</span></span> <span data-ttu-id="76b70-227">Tato stránka obsahuje registrační formulář pro registraci pro místní účet, který je založený na e-mailovou adresu nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="76b70-227">This page contains a sign-up form for signing up for a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="76b70-228">Hello formulář může obsahovat různé vstupní ovládací prvky, jako je například vstupní textové pole, pole pro zadání hesla, přepínače, pole rozevíracího seznamu vyberte jeden a vybrat víc zaškrtávací políčka.</span><span class="sxs-lookup"><span data-stu-id="76b70-228">hello form can contain various input controls, such as a text input box, a password entry box, a radio button, single-select drop-down boxes, and multi-select check boxes.</span></span> |
| <span data-ttu-id="76b70-229">*API.phonefactor*</span><span class="sxs-lookup"><span data-stu-id="76b70-229">*api.phonefactor*</span></span> | <span data-ttu-id="76b70-230">**Stránka služby Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="76b70-230">**Multi-factor authentication page**.</span></span> <span data-ttu-id="76b70-231">Na této stránce uživatelé mohli ověřit jejich telefonní čísla (pomocí textové nebo hlasové) během registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="76b70-231">On this page, users can verify their phone numbers (by using text or voice) during sign-up or sign-in.</span></span> |
| <span data-ttu-id="76b70-232">*API.selfasserted*</span><span class="sxs-lookup"><span data-stu-id="76b70-232">*api.selfasserted*</span></span> | <span data-ttu-id="76b70-233">**Stránku pro přihlášení sociálních účet**.</span><span class="sxs-lookup"><span data-stu-id="76b70-233">**Social account sign-up page**.</span></span> <span data-ttu-id="76b70-234">Tato stránka obsahuje registrační formulář, který uživatelé musí dokončit při registraci pomocí stávající účet od poskytovatele identity sociálních třeba Facebook nebo Google +.</span><span class="sxs-lookup"><span data-stu-id="76b70-234">This page contains a sign-up form that users must complete when they sign up by using an existing account from a social identity provider such as Facebook or Google+.</span></span> <span data-ttu-id="76b70-235">Tato stránka je podobné toohello předcházející sociálních účet stránku pro přihlášení, s výjimkou pole pro zadání hesla hello.</span><span class="sxs-lookup"><span data-stu-id="76b70-235">This page is similar toohello preceding social account sign-up page, except for hello password entry fields.</span></span> |
| <span data-ttu-id="76b70-236">*API.selfasserted.profileupdate*</span><span class="sxs-lookup"><span data-stu-id="76b70-236">*api.selfasserted.profileupdate*</span></span> | <span data-ttu-id="76b70-237">**Stránka pro aktualizaci profilu**.</span><span class="sxs-lookup"><span data-stu-id="76b70-237">**Profile update page**.</span></span> <span data-ttu-id="76b70-238">Tato stránka obsahuje formuláře, které uživatelé mohou používat tooupdate svůj profil.</span><span class="sxs-lookup"><span data-stu-id="76b70-238">This page contains a form that users can use tooupdate their profile.</span></span> <span data-ttu-id="76b70-239">Tato stránka je podobné toohello sociálních registrační stránku účtu, s výjimkou pole pro zadání hesla hello.</span><span class="sxs-lookup"><span data-stu-id="76b70-239">This page is similar toohello social account sign-up page, except for hello password entry fields.</span></span> |
| <span data-ttu-id="76b70-240">*API.signuporsignin*</span><span class="sxs-lookup"><span data-stu-id="76b70-240">*api.signuporsignin*</span></span> | <span data-ttu-id="76b70-241">**Jednotná stránku registrace nebo přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="76b70-241">**Unified sign-up or sign-in page**.</span></span> <span data-ttu-id="76b70-242">Tato stránka zpracuje obě hello registrace a přihlašování uživatelů, kteří můžou využívat poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook nebo Google + nebo místní účty.</span><span class="sxs-lookup"><span data-stu-id="76b70-242">This page handles both hello sign-up and sign-in of users, who can use enterprise identity providers, social identity providers such as Facebook or Google+, or local accounts.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="76b70-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76b70-243">Next steps</span></span>

<span data-ttu-id="76b70-244">Další informace o prvky uživatelského rozhraní, které se dají přizpůsobit najdete v tématu [referenční příručka pro přizpůsobení uživatelského rozhraní pro předdefinované zásady](active-directory-b2c-reference-ui-customization.md).</span><span class="sxs-lookup"><span data-stu-id="76b70-244">For additional information about UI elements that can be customized, see [reference guide for UI customization for built-in policies](active-directory-b2c-reference-ui-customization.md).</span></span>
