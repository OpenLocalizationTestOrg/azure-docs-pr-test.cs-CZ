---
title: "Zásady ověřování Azure API Management | Microsoft Docs"
description: "Další informace o zásady ověřování, která je k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 63ef20a56ab7721f9ecc7025d05963cc4b0c27a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-authentication-policies"></a><span data-ttu-id="5a8ad-103">Zásady ověřování služby API Management</span><span class="sxs-lookup"><span data-stu-id="5a8ad-103">API Management authentication policies</span></span>
<span data-ttu-id="5a8ad-104">Toto téma obsahuje odkaz pro následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="5a8ad-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="5a8ad-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="5a8ad-106"><a name="AuthenticationPolicies"></a>Zásady ověřování</span><span class="sxs-lookup"><span data-stu-id="5a8ad-106"><a name="AuthenticationPolicies"></a> Authentication policies</span></span>  
  
-   <span data-ttu-id="5a8ad-107">[Ověřování Basic](api-management-authentication-policies.md#Basic) -ověření pomocí back-end službu pomocí základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-107">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
-   <span data-ttu-id="5a8ad-108">[Ověřování pomocí certifikátu klienta](api-management-authentication-policies.md#ClientCertificate) -ověření pomocí back-end službu pomocí klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-108">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
##  <span data-ttu-id="5a8ad-109"><a name="Basic"></a>Ověřování Basic</span><span class="sxs-lookup"><span data-stu-id="5a8ad-109"><a name="Basic"></a> Authenticate with Basic</span></span>  
 <span data-ttu-id="5a8ad-110">Použití `authentication-basic` zásad k ověření s back-end službu pomocí základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-110">Use the `authentication-basic` policy to authenticate with a backend service using Basic authentication.</span></span> <span data-ttu-id="5a8ad-111">Tato zásada nastavuje hlavičku autorizace protokolu HTTP na hodnotu odpovídající přihlašovacích údajů zadaných v zásadách.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-111">This policy effectively sets the HTTP Authorization header to the value corresponding to the credentials provided in the policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="5a8ad-112">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="5a8ad-112">Policy statement</span></span>  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a><span data-ttu-id="5a8ad-113">Příklad</span><span class="sxs-lookup"><span data-stu-id="5a8ad-113">Example</span></span>  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a><span data-ttu-id="5a8ad-114">Elementy</span><span class="sxs-lookup"><span data-stu-id="5a8ad-114">Elements</span></span>  
  
|<span data-ttu-id="5a8ad-115">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5a8ad-115">Name</span></span>|<span data-ttu-id="5a8ad-116">Popis</span><span class="sxs-lookup"><span data-stu-id="5a8ad-116">Description</span></span>|<span data-ttu-id="5a8ad-117">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5a8ad-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="5a8ad-118">ověřování – základní</span><span class="sxs-lookup"><span data-stu-id="5a8ad-118">authentication-basic</span></span>|<span data-ttu-id="5a8ad-119">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-119">Root element.</span></span>|<span data-ttu-id="5a8ad-120">Ano</span><span class="sxs-lookup"><span data-stu-id="5a8ad-120">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="5a8ad-121">Atributy</span><span class="sxs-lookup"><span data-stu-id="5a8ad-121">Attributes</span></span>  
  
|<span data-ttu-id="5a8ad-122">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5a8ad-122">Name</span></span>|<span data-ttu-id="5a8ad-123">Popis</span><span class="sxs-lookup"><span data-stu-id="5a8ad-123">Description</span></span>|<span data-ttu-id="5a8ad-124">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5a8ad-124">Required</span></span>|<span data-ttu-id="5a8ad-125">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5a8ad-125">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="5a8ad-126">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5a8ad-126">username</span></span>|<span data-ttu-id="5a8ad-127">Určuje uživatelské jméno základní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-127">Specifies the username of the Basic credential.</span></span>|<span data-ttu-id="5a8ad-128">Ano</span><span class="sxs-lookup"><span data-stu-id="5a8ad-128">Yes</span></span>|<span data-ttu-id="5a8ad-129">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5a8ad-129">N/A</span></span>|  
|<span data-ttu-id="5a8ad-130">heslo</span><span class="sxs-lookup"><span data-stu-id="5a8ad-130">password</span></span>|<span data-ttu-id="5a8ad-131">Určuje heslo základní pověření.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-131">Specifies the password of the Basic credential.</span></span>|<span data-ttu-id="5a8ad-132">Ano</span><span class="sxs-lookup"><span data-stu-id="5a8ad-132">Yes</span></span>|<span data-ttu-id="5a8ad-133">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5a8ad-133">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="5a8ad-134">Využití</span><span class="sxs-lookup"><span data-stu-id="5a8ad-134">Usage</span></span>  
 <span data-ttu-id="5a8ad-135">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="5a8ad-135">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="5a8ad-136">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="5a8ad-136">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="5a8ad-137">**Zásady obory:** rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5a8ad-137">**Policy scopes:** API</span></span>  
  
##  <span data-ttu-id="5a8ad-138"><a name="ClientCertificate"></a>Ověřování pomocí certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="5a8ad-138"><a name="ClientCertificate"></a> Authenticate with client certificate</span></span>  
 <span data-ttu-id="5a8ad-139">Použití `authentication-certificate` zásad k ověření s back-end službu pomocí klientského certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-139">Use the `authentication-certificate` policy to authenticate with a backend service using client certificate.</span></span> <span data-ttu-id="5a8ad-140">Certifikát musí být [nainstalován do rozhraní API správy](http://go.microsoft.com/fwlink/?LinkID=511599) první a je určený podle jeho kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-140">The certificate needs to be [installed into API Management](http://go.microsoft.com/fwlink/?LinkID=511599) first and is identified by its thumbprint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="5a8ad-141">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="5a8ad-141">Policy statement</span></span>  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a><span data-ttu-id="5a8ad-142">Příklad</span><span class="sxs-lookup"><span data-stu-id="5a8ad-142">Example</span></span>  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a><span data-ttu-id="5a8ad-143">Elementy</span><span class="sxs-lookup"><span data-stu-id="5a8ad-143">Elements</span></span>  
  
|<span data-ttu-id="5a8ad-144">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5a8ad-144">Name</span></span>|<span data-ttu-id="5a8ad-145">Popis</span><span class="sxs-lookup"><span data-stu-id="5a8ad-145">Description</span></span>|<span data-ttu-id="5a8ad-146">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5a8ad-146">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="5a8ad-147">ověřovací certifikát</span><span class="sxs-lookup"><span data-stu-id="5a8ad-147">authentication-certificate</span></span>|<span data-ttu-id="5a8ad-148">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-148">Root element.</span></span>|<span data-ttu-id="5a8ad-149">Ano</span><span class="sxs-lookup"><span data-stu-id="5a8ad-149">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="5a8ad-150">Atributy</span><span class="sxs-lookup"><span data-stu-id="5a8ad-150">Attributes</span></span>  
  
|<span data-ttu-id="5a8ad-151">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5a8ad-151">Name</span></span>|<span data-ttu-id="5a8ad-152">Popis</span><span class="sxs-lookup"><span data-stu-id="5a8ad-152">Description</span></span>|<span data-ttu-id="5a8ad-153">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5a8ad-153">Required</span></span>|<span data-ttu-id="5a8ad-154">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5a8ad-154">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="5a8ad-155">Kryptografický otisk</span><span class="sxs-lookup"><span data-stu-id="5a8ad-155">thumbprint</span></span>|<span data-ttu-id="5a8ad-156">Kryptografický otisk certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="5a8ad-156">The thumbprint for the client certificate.</span></span>|<span data-ttu-id="5a8ad-157">Ano</span><span class="sxs-lookup"><span data-stu-id="5a8ad-157">Yes</span></span>|<span data-ttu-id="5a8ad-158">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5a8ad-158">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="5a8ad-159">Využití</span><span class="sxs-lookup"><span data-stu-id="5a8ad-159">Usage</span></span>  
 <span data-ttu-id="5a8ad-160">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="5a8ad-160">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="5a8ad-161">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="5a8ad-161">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="5a8ad-162">**Zásady obory:** rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5a8ad-162">**Policy scopes:** API</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="5a8ad-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a8ad-163">Next steps</span></span>
<span data-ttu-id="5a8ad-164">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="5a8ad-164">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  