---
title: "Zakódování nebo dekódování plochých souborů v Azure logic apps | Microsoft Docs"
description: "Použití souboru souboru kodér nebo dekodér v podniku integrační balíček ve vašich logic apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; mandia
ms.openlocfilehash: bc3430624844cdeb92958433fba295f67a8ae0ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a><span data-ttu-id="8013f-103">Přehled integrace enterprise s plochých souborů</span><span class="sxs-lookup"><span data-stu-id="8013f-103">Overview of enterprise integration with flat files</span></span>

<span data-ttu-id="8013f-104">Můžete se ke kódování obsahu XML před jejím odesláním obchodním partnerům ve scénáři business-to-business (B2B).</span><span class="sxs-lookup"><span data-stu-id="8013f-104">You may want to encode XML content before you send it to a business partner in a business-to-business (B2B) scenario.</span></span> <span data-ttu-id="8013f-105">V aplikaci logiky můžete k tomu plochý soubor kódování konektor.</span><span class="sxs-lookup"><span data-stu-id="8013f-105">In a logic app, you can use the flat file encoding connector to do this.</span></span> <span data-ttu-id="8013f-106">Aplikace logiky, který vytvoříte může získat jeho XML obsah z různých zdrojů, včetně aktivační procedury pro požadavek HTTP, z jiné aplikace nebo dokonce od dalších [konektory](../connectors/apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="8013f-106">The logic app that you create can get its XML content from a variety of sources, including from an HTTP request trigger, from another application, or even from one of the many [connectors](../connectors/apis-list.md).</span></span> <span data-ttu-id="8013f-107">Další informace o aplikacích logiky najdete v tématu [dokumentaci aplikace logiky](logic-apps-what-are-logic-apps.md "Další informace o aplikacích logiky").</span><span class="sxs-lookup"><span data-stu-id="8013f-107">For more information about logic apps, see the [logic apps documentation](logic-apps-what-are-logic-apps.md "Learn more about Logic apps").</span></span>  

## <a name="create-the-flat-file-encoding-connector"></a><span data-ttu-id="8013f-108">Vytvořit plochý soubor kódování konektoru</span><span class="sxs-lookup"><span data-stu-id="8013f-108">Create the flat file encoding connector</span></span>
<span data-ttu-id="8013f-109">Postupujte podle těchto kroků přidejte plochý soubor kódování konektoru pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="8013f-109">Follow these steps to add a flat file encoding connector to your logic app.</span></span>

1. <span data-ttu-id="8013f-110">Vytvoření aplikace logiky a [ho propojit se svým účtem integrace](logic-apps-enterprise-integration-accounts.md "zjistěte, jak lze propojit účet integrace aplikace logiky").</span><span class="sxs-lookup"><span data-stu-id="8013f-110">Create a logic app and [link it to your integration account](logic-apps-enterprise-integration-accounts.md "Learn to link an integration account to a Logic app").</span></span> <span data-ttu-id="8013f-111">Tento účet obsahuje schéma, které chcete použít ke kódování XML data.</span><span class="sxs-lookup"><span data-stu-id="8013f-111">This account contains the schema you will use to encode the XML data.</span></span>  
2. <span data-ttu-id="8013f-112">Přidat **požadavku - obdrží žádost HTTP při** aktivační svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="8013f-112">Add a **Request - When an HTTP request is received** trigger to your logic app.</span></span>  
   <span data-ttu-id="8013f-113">![Snímek obrazovky aktivační události a vyberte](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span><span class="sxs-lookup"><span data-stu-id="8013f-113">![Screenshot of trigger to select](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span></span>    
3. <span data-ttu-id="8013f-114">Přidejte plochý soubor kódování akce, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8013f-114">Add the flat file encoding action, as follows:</span></span>
   
    <span data-ttu-id="8013f-115">a.</span><span class="sxs-lookup"><span data-stu-id="8013f-115">a.</span></span> <span data-ttu-id="8013f-116">Vyberte **plus** přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8013f-116">Select the **plus** sign.</span></span>
   
    <span data-ttu-id="8013f-117">b.</span><span class="sxs-lookup"><span data-stu-id="8013f-117">b.</span></span> <span data-ttu-id="8013f-118">Vyberte **přidat akci** odkaz (se zobrazí po výběru na symbol plus).</span><span class="sxs-lookup"><span data-stu-id="8013f-118">Select the **Add an action** link (appears after you have selected the plus sign).</span></span>
   
    <span data-ttu-id="8013f-119">c.</span><span class="sxs-lookup"><span data-stu-id="8013f-119">c.</span></span> <span data-ttu-id="8013f-120">Do vyhledávacího pole zadejte *ploché* vyfiltrujete všechny akce, které ten, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="8013f-120">In the search box, enter *Flat* to filter all the actions to the one that you want to use.</span></span>
   
    <span data-ttu-id="8013f-121">d.</span><span class="sxs-lookup"><span data-stu-id="8013f-121">d.</span></span> <span data-ttu-id="8013f-122">Vyberte **ploché kódování souborů** možnost ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="8013f-122">Select the **Flat File Encoding** option from the list.</span></span>   
   <span data-ttu-id="8013f-123">![Možnost snímek z plochých kódování souborů](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span><span class="sxs-lookup"><span data-stu-id="8013f-123">![Screenshot of Flat File Encoding option](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span></span>   
4. <span data-ttu-id="8013f-124">Na **ploché kódování souborů** dialogové okno, vyberte **obsahu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8013f-124">On the **Flat File Encoding** dialog box, select the **Content** text box.</span></span>  
   <span data-ttu-id="8013f-125">![Snímek obrazovky obsahu textového pole](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)</span><span class="sxs-lookup"><span data-stu-id="8013f-125">![Screenshot of Content text box](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)</span></span>  
5. <span data-ttu-id="8013f-126">Vyberte značky body jako obsah, který chcete kódovat.</span><span class="sxs-lookup"><span data-stu-id="8013f-126">Select the body tag as the content that you want to encode.</span></span> <span data-ttu-id="8013f-127">Značky body naplní pole obsahu.</span><span class="sxs-lookup"><span data-stu-id="8013f-127">The body tag will populate the content field.</span></span>     
   ![Snímek obrazovky značky textu](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
6. <span data-ttu-id="8013f-129">Vyberte **název schématu** seznam pole a zvolte schématu, kterou chcete použít ke kódování vstupní obsah.</span><span class="sxs-lookup"><span data-stu-id="8013f-129">Select the **Schema Name** list box, and choose the schema you want to use to encode the input content.</span></span>    
   <span data-ttu-id="8013f-130">![Pole se seznamem název schématu snímek obrazovky](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)</span><span class="sxs-lookup"><span data-stu-id="8013f-130">![Screenshot of Schema Name list box](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)</span></span>  
7. <span data-ttu-id="8013f-131">Uložte práci.</span><span class="sxs-lookup"><span data-stu-id="8013f-131">Save your work.</span></span>   
   ![Snímek obrazovky uložit ikonu](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

<span data-ttu-id="8013f-133">V tomto okamžiku jste dokončili nastavení kódování konektor vaše plochý soubor.</span><span class="sxs-lookup"><span data-stu-id="8013f-133">At this point, you are finished setting up your flat file encoding connector.</span></span> <span data-ttu-id="8013f-134">V reálném světě aplikaci můžete data uložit do kódovaného-obchodní aplikaci, například služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="8013f-134">In a real world application, you may want to store the encoded data in a line-of-business application, such as Salesforce.</span></span> <span data-ttu-id="8013f-135">Nebo můžete odeslat, aby kódovaného dat obchodních partnerů.</span><span class="sxs-lookup"><span data-stu-id="8013f-135">Or you can send that encoded data to a trading partner.</span></span> <span data-ttu-id="8013f-136">Můžete snadno přidat akci pro odeslání výstupu kódování akce Salesforce nebo obchodního partnera, pomocí jedné jiných konektorů poskytuje.</span><span class="sxs-lookup"><span data-stu-id="8013f-136">You can easily add an action to send the output of the encoding action to Salesforce, or to your trading partner, by using any one of the other connectors provided.</span></span>

<span data-ttu-id="8013f-137">Nyní můžete otestovat váš konektor tím, že požadavek na koncový bod HTTP a včetně obsah XML v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="8013f-137">You can now test your connector by making a request to the HTTP endpoint, and including the XML content in the body of the request.</span></span>  

## <a name="create-the-flat-file-decoding-connector"></a><span data-ttu-id="8013f-138">Vytvořit plochý soubor dekódování konektoru</span><span class="sxs-lookup"><span data-stu-id="8013f-138">Create the flat file decoding connector</span></span>

> [!NOTE]
> <span data-ttu-id="8013f-139">K provedení těchto kroků, musíte mít soubor schématu již odeslány do integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="8013f-139">To complete these steps, you need to have a schema file already uploaded into you integration account.</span></span>

1. <span data-ttu-id="8013f-140">Přidat **požadavku - obdrží žádost HTTP při** aktivační svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="8013f-140">Add a **Request - When an HTTP request is received** trigger to your logic app.</span></span>  
   <span data-ttu-id="8013f-141">![Snímek obrazovky aktivační události a vyberte](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span><span class="sxs-lookup"><span data-stu-id="8013f-141">![Screenshot of trigger to select](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span></span>    
2. <span data-ttu-id="8013f-142">Přidejte plochý soubor dekódování akce, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8013f-142">Add the flat file decoding action, as follows:</span></span>
   
    <span data-ttu-id="8013f-143">a.</span><span class="sxs-lookup"><span data-stu-id="8013f-143">a.</span></span> <span data-ttu-id="8013f-144">Vyberte **plus** přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8013f-144">Select the **plus** sign.</span></span>
   
    <span data-ttu-id="8013f-145">b.</span><span class="sxs-lookup"><span data-stu-id="8013f-145">b.</span></span> <span data-ttu-id="8013f-146">Vyberte **přidat akci** odkaz (se zobrazí po výběru na symbol plus).</span><span class="sxs-lookup"><span data-stu-id="8013f-146">Select the **Add an action** link (appears after you have selected the plus sign).</span></span>
   
    <span data-ttu-id="8013f-147">c.</span><span class="sxs-lookup"><span data-stu-id="8013f-147">c.</span></span> <span data-ttu-id="8013f-148">Do vyhledávacího pole zadejte *ploché* vyfiltrujete všechny akce, které ten, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="8013f-148">In the search box, enter *Flat* to filter all the actions to the one that you want to use.</span></span>
   
    <span data-ttu-id="8013f-149">d.</span><span class="sxs-lookup"><span data-stu-id="8013f-149">d.</span></span> <span data-ttu-id="8013f-150">Vyberte **plochý soubor dekódování** možnost ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="8013f-150">Select the **Flat File Decoding** option from the list.</span></span>   
   <span data-ttu-id="8013f-151">![Snímek obrazovky z plochých dekódování souboru možnost](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span><span class="sxs-lookup"><span data-stu-id="8013f-151">![Screenshot of Flat File Decoding option](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span></span>   
3. <span data-ttu-id="8013f-152">Vyberte **obsahu** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="8013f-152">Select the **Content** control.</span></span> <span data-ttu-id="8013f-153">Tímto se vytvoří seznam obsahu z dřívějších krocích, které můžete použít jako obsah dekódovat.</span><span class="sxs-lookup"><span data-stu-id="8013f-153">This produces a list of the content from earlier steps that you can use as the content to decode.</span></span> <span data-ttu-id="8013f-154">Všimněte si, že *textu* z příchozí HTTP je k dispozici pro použití jako obsah dekódování se žádost.</span><span class="sxs-lookup"><span data-stu-id="8013f-154">Notice that the *Body* from the incoming HTTP request is available to be used as the content to decode.</span></span> <span data-ttu-id="8013f-155">Můžete také zadat obsah pro dekódování přímo do **obsah** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="8013f-155">You can also enter the content to decode directly into the **Content** control.</span></span>     
4. <span data-ttu-id="8013f-156">Vyberte *textu* značky.</span><span class="sxs-lookup"><span data-stu-id="8013f-156">Select the *Body* tag.</span></span> <span data-ttu-id="8013f-157">Všimněte si značky body je teď ve **obsahu** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="8013f-157">Notice the body tag is now in the **Content** control.</span></span>
5. <span data-ttu-id="8013f-158">Vyberte název schématu, který chcete použít k dekódování obsah.</span><span class="sxs-lookup"><span data-stu-id="8013f-158">Select the name of the schema that you want to use to decode the content.</span></span> <span data-ttu-id="8013f-159">Následující snímek obrazovky ukazuje, že *OrderFile* je název vybrané schéma.</span><span class="sxs-lookup"><span data-stu-id="8013f-159">The following screenshot shows that *OrderFile* is the selected schema name.</span></span> <span data-ttu-id="8013f-160">Tento název schématu měl nahraný v úvahu integrace dříve.</span><span class="sxs-lookup"><span data-stu-id="8013f-160">This schema name had been uploaded into the integration account previously.</span></span>
   
   ![Dialogové okno snímek obrazovky z plochých dekódování souboru](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
6. <span data-ttu-id="8013f-162">Uložte práci.</span><span class="sxs-lookup"><span data-stu-id="8013f-162">Save your work.</span></span>  
   ![Snímek obrazovky uložit ikonu](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

<span data-ttu-id="8013f-164">V tomto okamžiku jste dokončili nastavení plochého souboru dekódování konektor.</span><span class="sxs-lookup"><span data-stu-id="8013f-164">At this point, you are finished setting up your flat file decoding connector.</span></span> <span data-ttu-id="8013f-165">V reálné aplikaci můžete ukládat dekódovaná data v řádku obchodní aplikaci, například služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="8013f-165">In a real world application, you may want to store the decoded data in a line-of-business application such as Salesforce.</span></span> <span data-ttu-id="8013f-166">Můžete snadno přidat akci pro odeslání výstupu dekódování akce do služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="8013f-166">You can easily add an action to send the output of the decoding action to Salesforce.</span></span>

<span data-ttu-id="8013f-167">Nyní můžete otestovat váš konektor tím, že požadavek na koncový bod HTTP a včetně obsah XML, který chcete dekódovat v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="8013f-167">You can now test your connector by making a request to the HTTP endpoint and including the XML content you want to decode in the body of the request.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="8013f-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8013f-168">Next steps</span></span>
* <span data-ttu-id="8013f-169">[Další informace o integračního balíčku Enterprise](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku").</span><span class="sxs-lookup"><span data-stu-id="8013f-169">[Learn more about the Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about Enterprise Integration Pack").</span></span>  

