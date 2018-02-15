---
title: "Přizpůsobení uživatelského rozhraní pomocí vlastních zásad – Azure AD B2C | Microsoft Docs"
description: "Další informace o přizpůsobení uživatelského rozhraní (UI), zatímco pomocí vlastních zásad v Azure AD B2C."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: c430b488016f038ed1d7a67a8d52c057df1ea40e
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a><span data-ttu-id="3a30a-103">Azure Active Directory B2C: Konfigurace ve vlastních zásadách pro přizpůsobení uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3a30a-103">Azure Active Directory B2C: Configure UI customization in a custom policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="3a30a-104">Po dokončení tohoto článku, budete mít vlastní zásady registrace a přihlášení pomocí značky a vzhled.</span><span class="sxs-lookup"><span data-stu-id="3a30a-104">After you complete this article, you will have a sign-up and sign-in custom policy with your brand and appearance.</span></span> <span data-ttu-id="3a30a-105">S Azure Active Directory B2C (Azure AD B2C), můžete získat téměř plnou kontrolu nad obsah HTML a CSS, který se zobrazí uživatelům.</span><span class="sxs-lookup"><span data-stu-id="3a30a-105">With Azure Active Directory B2C (Azure AD B2C), you get nearly full control of the HTML and CSS content that's presented to users.</span></span> <span data-ttu-id="3a30a-106">Pokud používáte vlastní zásadu, nakonfigurujete přizpůsobení uživatelského rozhraní v kódu XML místo použití ovládacích prvků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3a30a-106">When you use a custom policy, you configure UI customization in XML instead of using controls in the Azure portal.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="3a30a-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3a30a-107">Prerequisites</span></span>

<span data-ttu-id="3a30a-108">Než začnete, dokončení [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="3a30a-108">Before you begin, complete [Getting started with custom policies](active-directory-b2c-get-started-custom.md).</span></span> <span data-ttu-id="3a30a-109">Měli byste mít pracovní vlastní zásadu pro registraci a přihlašování s místní účty.</span><span class="sxs-lookup"><span data-stu-id="3a30a-109">You should have a working custom policy for sign-up and sign-in with local accounts.</span></span>

## <a name="page-ui-customization"></a><span data-ttu-id="3a30a-110">Přizpůsobení uživatelského rozhraní stránky</span><span class="sxs-lookup"><span data-stu-id="3a30a-110">Page UI customization</span></span>

<span data-ttu-id="3a30a-111">Pomocí funkce přizpůsobení uživatelského rozhraní stránky můžete přizpůsobit vzhled a chování žádné vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="3a30a-111">By using the page UI customization feature, you can customize the look and feel of any custom policy.</span></span> <span data-ttu-id="3a30a-112">Můžete také udržovat značky a visual konzistenci mezi aplikací a Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="3a30a-112">You can also maintain brand and visual consistency between your application and Azure AD B2C.</span></span>

<span data-ttu-id="3a30a-113">Zde je, jak to funguje: Azure AD B2C spuštěním kódu v prohlížeči vašeho zákazníka a používá moderní přístup názvem [sdílení prostředků různých původů (CORS)](http://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="3a30a-113">Here's how it works: Azure AD B2C runs code in your customer's browser and uses a modern approach called [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span></span> <span data-ttu-id="3a30a-114">Nejdřív zadejte adresu URL ve vlastních zásadách s přizpůsobený obsah HTML.</span><span class="sxs-lookup"><span data-stu-id="3a30a-114">First, you specify a URL in the custom policy with customized HTML content.</span></span> <span data-ttu-id="3a30a-115">Azure AD B2C sloučí elementy uživatelského rozhraní pomocí obsah HTML, který je načten z vaší adresy URL a potom zobrazí stránku zákazník.</span><span class="sxs-lookup"><span data-stu-id="3a30a-115">Azure AD B2C merges UI elements with the HTML content that's loaded from your URL and then displays the page to the customer.</span></span>

## <a name="create-your-html5-content"></a><span data-ttu-id="3a30a-116">Vytvoření vaší HTML5 obsahu</span><span class="sxs-lookup"><span data-stu-id="3a30a-116">Create your HTML5 content</span></span>

<span data-ttu-id="3a30a-117">Vytváření obsahu s názvem vašeho produktu značky HTML v názvu.</span><span class="sxs-lookup"><span data-stu-id="3a30a-117">Create HTML content with your product's brand name in the title.</span></span>

1. <span data-ttu-id="3a30a-118">Zkopírujte následující fragment kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="3a30a-118">Copy the following HTML snippet.</span></span> <span data-ttu-id="3a30a-119">Je ve správném formátu názvem HTML5 s prázdný element  *\<div id = "api"\>\</div\>*  umístěné v rámci  *\<textu\>*  značky.</span><span class="sxs-lookup"><span data-stu-id="3a30a-119">It is well-formed HTML5 with an empty element called *\<div id="api"\>\</div\>* located within the *\<body\>* tags.</span></span> <span data-ttu-id="3a30a-120">Tento element určuje, kde Azure AD B2C obsah má být vložen.</span><span class="sxs-lookup"><span data-stu-id="3a30a-120">This element indicates where Azure AD B2C content is to be inserted.</span></span>

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
   ><span data-ttu-id="3a30a-121">Z bezpečnostních důvodů je používání jazyka JavaScript aktuálně blokován pro přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="3a30a-121">For security reasons, the use of JavaScript is currently blocked for customization.</span></span>

2. <span data-ttu-id="3a30a-122">Vložte zkopírovaný fragment v textovém editoru a pak soubor uložte jako *přizpůsobit ui.html*.</span><span class="sxs-lookup"><span data-stu-id="3a30a-122">Paste the copied snippet in a text editor, and then save the file as *customize-ui.html*.</span></span>

## <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="3a30a-123">Vytvoření účtu úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="3a30a-123">Create an Azure Blob storage account</span></span>

>[!NOTE]
> <span data-ttu-id="3a30a-124">V tomto článku používáme úložiště objektů Blob v Azure k hostování náš obsah.</span><span class="sxs-lookup"><span data-stu-id="3a30a-124">In this article, we use Azure Blob storage to host our content.</span></span> <span data-ttu-id="3a30a-125">Je možné hostovat obsah na webovém serveru, ale musíte [povolení CORS na vašem webovém serveru](https://enable-cors.org/server.html).</span><span class="sxs-lookup"><span data-stu-id="3a30a-125">You can choose to host your content on a web server, but you must [enable CORS on your web server](https://enable-cors.org/server.html).</span></span>

<span data-ttu-id="3a30a-126">Chcete-li hostovat tento obsah HTML v úložišti objektů Blob, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="3a30a-126">To host this HTML content in Blob storage, do the following:</span></span>

1. <span data-ttu-id="3a30a-127">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3a30a-127">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3a30a-128">Na **rozbočovače** nabídce vyberte možnost **nový** > **úložiště** > **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-128">On the **Hub** menu, select **New** > **Storage** > **Storage account**.</span></span>
3. <span data-ttu-id="3a30a-129">Zadejte jedinečný **název** pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3a30a-129">Enter a unique **Name** for your storage account.</span></span>
4. <span data-ttu-id="3a30a-130">**Model nasazení** může zůstat **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-130">**Deployment model** can remain **Resource Manager**.</span></span>
5. <span data-ttu-id="3a30a-131">Změna **druh účtu** k **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-131">Change **Account Kind** to **Blob storage**.</span></span>
6. <span data-ttu-id="3a30a-132">**Výkon** může zůstat **standardní**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-132">**Performance** can remain **Standard**.</span></span>
7. <span data-ttu-id="3a30a-133">**Replikace** může zůstat **RA-GRS**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-133">**Replication** can remain **RA-GRS**.</span></span>
8. <span data-ttu-id="3a30a-134">**Úroveň přístupu** může zůstat **horká**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-134">**Access tier** can remain **Hot**.</span></span>
9. <span data-ttu-id="3a30a-135">**Šifrování služby úložiště** může zůstat **zakázané**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-135">**Storage service encryption** can remain **Disabled**.</span></span>
10. <span data-ttu-id="3a30a-136">Vyberte **předplatné** pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3a30a-136">Select a **Subscription** for your storage account.</span></span>
11. <span data-ttu-id="3a30a-137">Vytvoření **skupiny prostředků** nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="3a30a-137">Create a **Resource group** or select an existing one.</span></span>
12. <span data-ttu-id="3a30a-138">Vyberte **zeměpisné polohy** pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3a30a-138">Select the **Geographic location** for your storage account.</span></span>
13. <span data-ttu-id="3a30a-139">Vytvořte účet úložiště kliknutím na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-139">Click **Create** to create the storage account.</span></span>  
    <span data-ttu-id="3a30a-140">Po dokončení nasazení **účet úložiště** automaticky otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="3a30a-140">After the deployment is completed, the **Storage account** blade opens automatically.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="3a30a-141">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="3a30a-141">Create a container</span></span>

<span data-ttu-id="3a30a-142">Pokud chcete vytvořit veřejném kontejneru v úložiště objektů Blob, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="3a30a-142">To create a public container in Blob storage, do the following:</span></span>

1. <span data-ttu-id="3a30a-143">Klikněte **přehled** kartě.</span><span class="sxs-lookup"><span data-stu-id="3a30a-143">Click the **Overview** tab.</span></span>
2. <span data-ttu-id="3a30a-144">Klikněte na tlačítko **kontejneru**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-144">Click **Container**.</span></span>
3. <span data-ttu-id="3a30a-145">Pro **název**, typ **$root**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-145">For **Name**, type **$root**.</span></span>
4. <span data-ttu-id="3a30a-146">Nastavit **přistupovat typu** k **Blob**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-146">Set **Access type** to **Blob**.</span></span>
5. <span data-ttu-id="3a30a-147">Klikněte na tlačítko **$root** otevřete nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="3a30a-147">Click **$root** to open the new container.</span></span>
6. <span data-ttu-id="3a30a-148">Klikněte na **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-148">Click **Upload**.</span></span>
7. <span data-ttu-id="3a30a-149">Klikněte na ikonu složky vedle **vyberte soubor**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-149">Click the folder icon next to **Select a file**.</span></span>
8. <span data-ttu-id="3a30a-150">Přejděte na **přizpůsobit ui.html**, který jste vytvořili předtím v [přizpůsobení uživatelského rozhraní stránky](#the-page-ui-customization-feature) části.</span><span class="sxs-lookup"><span data-stu-id="3a30a-150">Go to **customize-ui.html**, which you created earlier in the [Page UI customization](#the-page-ui-customization-feature) section.</span></span>
9. <span data-ttu-id="3a30a-151">Klikněte na **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-151">Click **Upload**.</span></span>
10. <span data-ttu-id="3a30a-152">Vyberte objekt blob přizpůsobit ui.html, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="3a30a-152">Select the customize-ui.html blob that you uploaded.</span></span>
11. <span data-ttu-id="3a30a-153">Vedle **URL**, klikněte na tlačítko **kopie**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-153">Next to **URL**, click **Copy**.</span></span>
12. <span data-ttu-id="3a30a-154">V prohlížeči vložte zkopírovanou adresu URL a přejděte na web.</span><span class="sxs-lookup"><span data-stu-id="3a30a-154">In a browser, paste the copied URL, and go to the site.</span></span> <span data-ttu-id="3a30a-155">Pokud je lokalita nedostupné, ujistěte se, že typ přístupu kontejneru je nastaven na **blob**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-155">If the site is inaccessible, make sure the container access type is set to **blob**.</span></span>

## <a name="configure-cors"></a><span data-ttu-id="3a30a-156">Konfigurace CORS</span><span class="sxs-lookup"><span data-stu-id="3a30a-156">Configure CORS</span></span>

<span data-ttu-id="3a30a-157">Konfigurace úložiště objektů Blob pro sdílení prostředků různého původu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3a30a-157">Configure Blob storage for Cross-Origin Resource Sharing by doing the following:</span></span>

>[!NOTE]
><span data-ttu-id="3a30a-158">Chcete vyzkoušet funkce přizpůsobení uživatelského rozhraní pomocí naše ukázka HTML a CSS obsahu?</span><span class="sxs-lookup"><span data-stu-id="3a30a-158">Want to try out the UI customization feature by using our sample HTML and CSS content?</span></span> <span data-ttu-id="3a30a-159">Nabízíme [jednoduché pomocný nástroj](active-directory-b2c-reference-ui-customization-helper-tool.md) , odešle a nakonfiguruje naše ukázkový obsah na vašem účtu úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="3a30a-159">We've provided [a simple helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) that uploads and configures our sample content on your Blob storage account.</span></span> <span data-ttu-id="3a30a-160">Pokud použijete nástroj, přeskočit na [upravte registrace nebo přihlášení vlastní zásady](#modify-your-sign-up-or-sign-in-custom-policy).</span><span class="sxs-lookup"><span data-stu-id="3a30a-160">If you use the tool, skip ahead to [Modify your sign-up or sign-in custom policy](#modify-your-sign-up-or-sign-in-custom-policy).</span></span>

1. <span data-ttu-id="3a30a-161">Na **úložiště** okno, v části **nastavení**, otevřete **CORS**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-161">On the **Storage** blade, under **Settings**, open **CORS**.</span></span>
2. <span data-ttu-id="3a30a-162">Klikněte na tlačítko **Add** (Přidat).</span><span class="sxs-lookup"><span data-stu-id="3a30a-162">Click **Add**.</span></span>
3. <span data-ttu-id="3a30a-163">Pro **povolené zdroje**, zadejte hvězdičku (\*).</span><span class="sxs-lookup"><span data-stu-id="3a30a-163">For **Allowed origins**, type an asterisk (\*).</span></span>
4. <span data-ttu-id="3a30a-164">V **povolených operací** rozevíracího seznamu, vyberte **získat** a **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-164">In the **Allowed verbs** drop-down list, select both **GET** and **OPTIONS**.</span></span>
5. <span data-ttu-id="3a30a-165">Pro **povolené hlavičky**, zadejte hvězdičku (\*).</span><span class="sxs-lookup"><span data-stu-id="3a30a-165">For **Allowed headers**, type an asterisk (\*).</span></span>
6. <span data-ttu-id="3a30a-166">Pro **zveřejněné hlavičky**, zadejte hvězdičku (\*).</span><span class="sxs-lookup"><span data-stu-id="3a30a-166">For **Exposed headers**, type an asterisk (\*).</span></span>
7. <span data-ttu-id="3a30a-167">Pro **maximální stáří (v sekundách)**, typ **200**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-167">For **Maximum age (seconds)**, type **200**.</span></span>
8. <span data-ttu-id="3a30a-168">Klikněte na tlačítko **Add** (Přidat).</span><span class="sxs-lookup"><span data-stu-id="3a30a-168">Click **Add**.</span></span>

## <a name="test-cors"></a><span data-ttu-id="3a30a-169">Test CORS</span><span class="sxs-lookup"><span data-stu-id="3a30a-169">Test CORS</span></span>

<span data-ttu-id="3a30a-170">Ověřte, že jste připraveni následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3a30a-170">Validate that you're ready by doing the following:</span></span>

1. <span data-ttu-id="3a30a-171">Přejděte na [test cors.org](http://test-cors.org/) webu a vložte adresu URL v **vzdálenou adresou URL** pole.</span><span class="sxs-lookup"><span data-stu-id="3a30a-171">Go to the [test-cors.org](http://test-cors.org/) website, and then paste the URL in the **Remote URL** box.</span></span>
2. <span data-ttu-id="3a30a-172">Klikněte na tlačítko **poslat žádost o**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-172">Click **Send Request**.</span></span>  
    <span data-ttu-id="3a30a-173">Pokud se zobrazí chyba, ujistěte se, že vaše [nastavení CORS](#configure-cors) jsou správné.</span><span class="sxs-lookup"><span data-stu-id="3a30a-173">If you receive an error, make sure that your [CORS settings](#configure-cors) are correct.</span></span> <span data-ttu-id="3a30a-174">Může se také muset vymazat mezipaměť prohlížeče nebo otevřete relaci procházení v privátní stisknutím kombinace kláves Ctrl + Shift + P.</span><span class="sxs-lookup"><span data-stu-id="3a30a-174">You might also need to clear your browser cache or open an in-private browsing session by pressing Ctrl+Shift+P.</span></span>

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a><span data-ttu-id="3a30a-175">Upravit vlastní zásady registrace nebo přihlášení</span><span class="sxs-lookup"><span data-stu-id="3a30a-175">Modify your sign-up or sign-in custom policy</span></span>

<span data-ttu-id="3a30a-176">V části nejvyšší úrovně  *\<TrustFrameworkPolicy\>*  značka, byste měli najít  *\<BuildingBlocks\>*  značky.</span><span class="sxs-lookup"><span data-stu-id="3a30a-176">Under the top-level *\<TrustFrameworkPolicy\>* tag, you should find *\<BuildingBlocks\>* tag.</span></span> <span data-ttu-id="3a30a-177">V rámci  *\<BuildingBlocks\>*  přidat značky,  *\<ContentDefinitions\>*  značky tak, že zkopírujete následující příklad.</span><span class="sxs-lookup"><span data-stu-id="3a30a-177">Within the *\<BuildingBlocks\>* tags, add a *\<ContentDefinitions\>* tag by copying the following example.</span></span> <span data-ttu-id="3a30a-178">Nahraďte *your_storage_account* s názvem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3a30a-178">Replace *your_storage_account* with the name of your storage account.</span></span>

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a><span data-ttu-id="3a30a-179">Nahrát váš aktualizované vlastní zásady</span><span class="sxs-lookup"><span data-stu-id="3a30a-179">Upload your updated custom policy</span></span>

1. <span data-ttu-id="3a30a-180">V [portál Azure](https://portal.azure.com), [přepnout do kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a pak otevřete **Azure AD B2C** okno.</span><span class="sxs-lookup"><span data-stu-id="3a30a-180">In the [Azure portal](https://portal.azure.com), [switch into the context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then open the **Azure AD B2C** blade.</span></span>
2. <span data-ttu-id="3a30a-181">Klikněte na tlačítko **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-181">Click **All Policies**.</span></span>
3. <span data-ttu-id="3a30a-182">Klikněte na tlačítko **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-182">Click **Upload Policy**.</span></span>
4. <span data-ttu-id="3a30a-183">Nahrát `SignUpOrSignin.xml` s  *\<ContentDefinitions\>*  značky, které jste přidali dříve.</span><span class="sxs-lookup"><span data-stu-id="3a30a-183">Upload `SignUpOrSignin.xml` with the *\<ContentDefinitions\>* tag that you added previously.</span></span>

## <a name="test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="3a30a-184">Otestovat pomocí vlastních zásad **spustit nyní**</span><span class="sxs-lookup"><span data-stu-id="3a30a-184">Test the custom policy by using **Run now**</span></span>

1. <span data-ttu-id="3a30a-185">Na **Azure AD B2C** okno, přejděte na **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-185">On the **Azure AD B2C** blade, go to **All polices**.</span></span>
2. <span data-ttu-id="3a30a-186">Vyberte vlastní zásady, který jste nahráli a klikněte na **spustit nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3a30a-186">Select the custom policy that you uploaded, and click the **Run now** button.</span></span>
3. <span data-ttu-id="3a30a-187">Nyní byste měli mít přihlásit pomocí e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="3a30a-187">You should be able to sign up by using an email address.</span></span>

## <a name="reference"></a><span data-ttu-id="3a30a-188">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="3a30a-188">Reference</span></span>

<span data-ttu-id="3a30a-189">Pro přizpůsobení uživatelského rozhraní Zde můžete najít ukázkové šablony:</span><span class="sxs-lookup"><span data-stu-id="3a30a-189">You can find sample templates for UI customization here:</span></span>

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

<span data-ttu-id="3a30a-190">Složka sample_templates/wingtip obsahuje následující soubory HTML:</span><span class="sxs-lookup"><span data-stu-id="3a30a-190">The sample_templates/wingtip folder contains the following HTML files:</span></span>

| <span data-ttu-id="3a30a-191">HTML5 šablony</span><span class="sxs-lookup"><span data-stu-id="3a30a-191">HTML5 template</span></span> | <span data-ttu-id="3a30a-192">Popis</span><span class="sxs-lookup"><span data-stu-id="3a30a-192">Description</span></span> |
|----------------|-------------|
| <span data-ttu-id="3a30a-193">*phonefactor.html*</span><span class="sxs-lookup"><span data-stu-id="3a30a-193">*phonefactor.html*</span></span> | <span data-ttu-id="3a30a-194">Tento soubor jako šablonu použijte pro stránku služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="3a30a-194">Use this file as a template for a multi-factor authentication page.</span></span> |
| <span data-ttu-id="3a30a-195">*resetpassword.html*</span><span class="sxs-lookup"><span data-stu-id="3a30a-195">*resetpassword.html*</span></span> | <span data-ttu-id="3a30a-196">Použít jako šablonu pro tento soubor zapomněli jste heslo?.</span><span class="sxs-lookup"><span data-stu-id="3a30a-196">Use this file as a template for a forgot password page.</span></span> |
| <span data-ttu-id="3a30a-197">*selfasserted.html*</span><span class="sxs-lookup"><span data-stu-id="3a30a-197">*selfasserted.html*</span></span> | <span data-ttu-id="3a30a-198">Tento soubor jako šablonu použijte pro stránku pro přihlášení sociálních účet, stránku pro přihlášení místní účet nebo na přihlašovací stránku místní účet.</span><span class="sxs-lookup"><span data-stu-id="3a30a-198">Use this file as a template for a social account sign-up page, a local account sign-up page, or a local account sign-in page.</span></span> |
| <span data-ttu-id="3a30a-199">*unified.html*</span><span class="sxs-lookup"><span data-stu-id="3a30a-199">*unified.html*</span></span> | <span data-ttu-id="3a30a-200">Tento soubor jako šablonu použijte pro jednotné stránku registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3a30a-200">Use this file as a template for a unified sign-up or sign-in page.</span></span> |
| <span data-ttu-id="3a30a-201">*updateprofile.html*</span><span class="sxs-lookup"><span data-stu-id="3a30a-201">*updateprofile.html*</span></span> | <span data-ttu-id="3a30a-202">Tento soubor jako šablonu použijte pro stránku aktualizace profilu.</span><span class="sxs-lookup"><span data-stu-id="3a30a-202">Use this file as a template for a profile update page.</span></span> |

<span data-ttu-id="3a30a-203">V [upravte část vaší vlastní zásady registrace nebo přihlášení](#modify-your-sign-up-or-sign-in-custom-policy), jste nakonfigurovali obsahu definice `api.idpselections`.</span><span class="sxs-lookup"><span data-stu-id="3a30a-203">In the [Modify your sign-up or sign-in custom policy section](#modify-your-sign-up-or-sign-in-custom-policy), you configured the content definition for `api.idpselections`.</span></span> <span data-ttu-id="3a30a-204">Kompletní obsah definice ID, která rozpoznává rozhraní Azure AD B2C identity prostředí a jejich popisy jsou v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="3a30a-204">The full set of content definition IDs that are recognized by the Azure AD B2C identity experience framework and their descriptions are in the following table:</span></span>

| <span data-ttu-id="3a30a-205">ID obsahu definice</span><span class="sxs-lookup"><span data-stu-id="3a30a-205">Content definition ID</span></span> | <span data-ttu-id="3a30a-206">Popis</span><span class="sxs-lookup"><span data-stu-id="3a30a-206">Description</span></span> | 
|-----------------------|-------------|
| <span data-ttu-id="3a30a-207">*api.error*</span><span class="sxs-lookup"><span data-stu-id="3a30a-207">*api.error*</span></span> | <span data-ttu-id="3a30a-208">**Chybové stránky**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-208">**Error page**.</span></span> <span data-ttu-id="3a30a-209">Tato stránka se zobrazí, když je došlo k výjimce nebo došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="3a30a-209">This page is displayed when an exception or an error is encountered.</span></span> |
| <span data-ttu-id="3a30a-210">*api.idpselections*</span><span class="sxs-lookup"><span data-stu-id="3a30a-210">*api.idpselections*</span></span> | <span data-ttu-id="3a30a-211">**Stránka Výběr zprostředkovatele identity**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-211">**Identity provider selection page**.</span></span> <span data-ttu-id="3a30a-212">Tato stránka obsahuje seznam zprostředkovatelů identity, které může uživatel vybírat během přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3a30a-212">This page contains a list of identity providers that the user can choose from during sign-in.</span></span> <span data-ttu-id="3a30a-213">Tyto možnosti jsou poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google + nebo místním účtům.</span><span class="sxs-lookup"><span data-stu-id="3a30a-213">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="3a30a-214">*api.idpselections.signup*</span><span class="sxs-lookup"><span data-stu-id="3a30a-214">*api.idpselections.signup*</span></span> | <span data-ttu-id="3a30a-215">**Výběr zprostředkovatele identity pro registraci**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-215">**Identity provider selection for sign-up**.</span></span> <span data-ttu-id="3a30a-216">Tato stránka obsahuje seznam poskytovatelů identity, které může uživatel vybírat během registrace.</span><span class="sxs-lookup"><span data-stu-id="3a30a-216">This page contains a list of identity providers that the user can choose from during sign-up.</span></span> <span data-ttu-id="3a30a-217">Tyto možnosti jsou poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google + nebo místním účtům.</span><span class="sxs-lookup"><span data-stu-id="3a30a-217">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="3a30a-218">*api.localaccountpasswordreset*</span><span class="sxs-lookup"><span data-stu-id="3a30a-218">*api.localaccountpasswordreset*</span></span> | <span data-ttu-id="3a30a-219">**Zapomněli jste heslo**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-219">**Forgot password page**.</span></span> <span data-ttu-id="3a30a-220">Tato stránka obsahuje formulář, který uživatel musí dokončit zahájíte resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="3a30a-220">This page contains a form that the user must complete to initiate a password reset.</span></span>  |
| <span data-ttu-id="3a30a-221">*api.localaccountsignin*</span><span class="sxs-lookup"><span data-stu-id="3a30a-221">*api.localaccountsignin*</span></span> | <span data-ttu-id="3a30a-222">**Přihlašovací stránka místní účet**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-222">**Local account sign-in page**.</span></span> <span data-ttu-id="3a30a-223">Tato stránka obsahuje formulář přihlášení pro přihlášení pomocí místního účtu, který je založený na e-mailovou adresu nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3a30a-223">This page contains a sign-in form for signing in with a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="3a30a-224">Formulář může obsahovat vstupní textové pole a pole pro zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="3a30a-224">The form can contain a text input box and password entry box.</span></span> |
| <span data-ttu-id="3a30a-225">*api.localaccountsignup*</span><span class="sxs-lookup"><span data-stu-id="3a30a-225">*api.localaccountsignup*</span></span> | <span data-ttu-id="3a30a-226">**Místní účet stránku**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-226">**Local account sign-up page**.</span></span> <span data-ttu-id="3a30a-227">Tato stránka obsahuje registrační formulář pro registraci pro místní účet, který je založený na e-mailovou adresu nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3a30a-227">This page contains a sign-up form for signing up for a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="3a30a-228">Formulář může obsahovat různé vstupní ovládací prvky, jako je například vstupní textové pole, pole pro zadání hesla, přepínače, pole rozevíracího seznamu vyberte jeden a vybrat víc zaškrtávací políčka.</span><span class="sxs-lookup"><span data-stu-id="3a30a-228">The form can contain various input controls, such as a text input box, a password entry box, a radio button, single-select drop-down boxes, and multi-select check boxes.</span></span> |
| <span data-ttu-id="3a30a-229">*api.phonefactor*</span><span class="sxs-lookup"><span data-stu-id="3a30a-229">*api.phonefactor*</span></span> | <span data-ttu-id="3a30a-230">**Stránka služby Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-230">**Multi-factor authentication page**.</span></span> <span data-ttu-id="3a30a-231">Na této stránce uživatelé mohli ověřit jejich telefonní čísla (pomocí textové nebo hlasové) během registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3a30a-231">On this page, users can verify their phone numbers (by using text or voice) during sign-up or sign-in.</span></span> |
| <span data-ttu-id="3a30a-232">*api.selfasserted*</span><span class="sxs-lookup"><span data-stu-id="3a30a-232">*api.selfasserted*</span></span> | <span data-ttu-id="3a30a-233">**Stránku pro přihlášení sociálních účet**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-233">**Social account sign-up page**.</span></span> <span data-ttu-id="3a30a-234">Tato stránka obsahuje registrační formulář, který uživatelé musí dokončit při registraci pomocí stávající účet od poskytovatele identity sociálních třeba Facebook nebo Google +.</span><span class="sxs-lookup"><span data-stu-id="3a30a-234">This page contains a sign-up form that users must complete when they sign up by using an existing account from a social identity provider such as Facebook or Google+.</span></span> <span data-ttu-id="3a30a-235">Tato stránka je podobný na předchozí sociálních registrační stránku účtu, s výjimkou pole pro zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="3a30a-235">This page is similar to the preceding social account sign-up page, except for the password entry fields.</span></span> |
| <span data-ttu-id="3a30a-236">*api.selfasserted.profileupdate*</span><span class="sxs-lookup"><span data-stu-id="3a30a-236">*api.selfasserted.profileupdate*</span></span> | <span data-ttu-id="3a30a-237">**Stránka pro aktualizaci profilu**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-237">**Profile update page**.</span></span> <span data-ttu-id="3a30a-238">Tato stránka obsahuje formulář, který uživatelé lze použít k aktualizaci svůj profil.</span><span class="sxs-lookup"><span data-stu-id="3a30a-238">This page contains a form that users can use to update their profile.</span></span> <span data-ttu-id="3a30a-239">Tato stránka je podobná registrační stránku sociálních účtu, s výjimkou pole pro zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="3a30a-239">This page is similar to the social account sign-up page, except for the password entry fields.</span></span> |
| <span data-ttu-id="3a30a-240">*api.signuporsignin*</span><span class="sxs-lookup"><span data-stu-id="3a30a-240">*api.signuporsignin*</span></span> | <span data-ttu-id="3a30a-241">**Jednotná stránku registrace nebo přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="3a30a-241">**Unified sign-up or sign-in page**.</span></span> <span data-ttu-id="3a30a-242">Tato stránka zpracovává registrace i přihlášení uživatelů, kteří můžou využívat poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook nebo Google + nebo místní účty.</span><span class="sxs-lookup"><span data-stu-id="3a30a-242">This page handles both the sign-up and sign-in of users, who can use enterprise identity providers, social identity providers such as Facebook or Google+, or local accounts.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="3a30a-243">Další postup</span><span class="sxs-lookup"><span data-stu-id="3a30a-243">Next steps</span></span>

<span data-ttu-id="3a30a-244">Další informace o prvky uživatelského rozhraní, které se dají přizpůsobit najdete v tématu [referenční příručka pro přizpůsobení uživatelského rozhraní pro předdefinované zásady](active-directory-b2c-reference-ui-customization.md).</span><span class="sxs-lookup"><span data-stu-id="3a30a-244">For additional information about UI elements that can be customized, see [reference guide for UI customization for built-in policies](active-directory-b2c-reference-ui-customization.md).</span></span>
