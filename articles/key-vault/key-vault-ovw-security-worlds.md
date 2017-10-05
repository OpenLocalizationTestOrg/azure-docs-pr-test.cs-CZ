---
ms.assetid: 
title: Azure Key Vault architektury security World | Microsoft Docs
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 921bbd109c9ea98d8b5c286a7512bed026412d26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a><span data-ttu-id="94009-102">Azure Key Vault architektury security World a geografické hranice.</span><span class="sxs-lookup"><span data-stu-id="94009-102">Azure Key Vault security worlds and geographic boundaries</span></span>

<span data-ttu-id="94009-103">Azure Key Vault je víceklientské služby a používá fond modulů hardwarového zabezpečení (HSM) v každém umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="94009-103">Azure Key Vault is a multi-tenant service and uses a pool of Hardware Security Modules (HSMs) in each Azure location.</span></span> 

<span data-ttu-id="94009-104">Všechny moduly hardwarového zabezpečení v Azure umístěních ve stejné geografické oblasti sdílet stejnou hranici kryptografických (Thales Security World).</span><span class="sxs-lookup"><span data-stu-id="94009-104">All HSMs at Azure locations in the same geographic region share the same cryptographic boundary (Thales Security World).</span></span> <span data-ttu-id="94009-105">Východ USA a západní USA sdílet stejnou architektury security world, protože náleží do US geografické umístění.</span><span class="sxs-lookup"><span data-stu-id="94009-105">For example, East US and West US share the same security world because they belong to the US geo location.</span></span> <span data-ttu-id="94009-106">Podobně všech Azure umístění v Japonsku sdílet stejnou architektury security world a všechny Azure umístění v Austrálii, Indie a tak dále.</span><span class="sxs-lookup"><span data-stu-id="94009-106">Similarly, all Azure locations in Japan share the same security world and all Azure locations in Australia, India, and so on.</span></span> 

## <a name="backup-and-restore-behavior"></a><span data-ttu-id="94009-107">Zálohování a obnovení chování</span><span class="sxs-lookup"><span data-stu-id="94009-107">Backup and restore behavior</span></span>

<span data-ttu-id="94009-108">Zálohy klíče z trezoru klíčů v jednom místě, Azure možné obnovit do trezoru klíčů v jiném umístění Azure, tak dlouho, dokud jsou splněny obě tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="94009-108">A backup taken of a key from a key vault in one Azure location can be restored to a key vault in another Azure location, as long as both of these conditions are true:</span></span>

- <span data-ttu-id="94009-109">Z Azure umístění patřit do stejného geografického umístění</span><span class="sxs-lookup"><span data-stu-id="94009-109">Both of the Azure locations belong to the same geographical location</span></span>
- <span data-ttu-id="94009-110">Obě trezorů klíčů patří do stejného předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="94009-110">Both of the key vaults belong to the same Azure subscription</span></span>

<span data-ttu-id="94009-111">Například zálohy podle konkrétního předplatného klíče v trezoru klíčů v Indie – Západ, lze obnovit pouze na jiné trezoru klíčů ve stejném předplatném a umístění geograficky; Indie – Západ, Indie centrální nebo – Jih, Indie.</span><span class="sxs-lookup"><span data-stu-id="94009-111">For example, a backup taken by a given subscription of a key in a key vault in West India, can only be restored to another key vault in the same subscription and geo location; West India, Central India or South India.</span></span>

## <a name="regions-and-products"></a><span data-ttu-id="94009-112">Oblasti a produkty</span><span class="sxs-lookup"><span data-stu-id="94009-112">Regions and products</span></span>

- [<span data-ttu-id="94009-113">Oblasti Azure</span><span class="sxs-lookup"><span data-stu-id="94009-113">Azure regions</span></span>](https://azure.microsoft.com/regions/)
- [<span data-ttu-id="94009-114">Produkty společnosti Microsoft podle oblasti</span><span class="sxs-lookup"><span data-stu-id="94009-114">Microsoft products by region</span></span>](https://azure.microsoft.com/regions/services/)

<span data-ttu-id="94009-115">Oblasti jsou namapované na architektury security World, zobrazené jako hlavní záhlaví v tabulkách:</span><span class="sxs-lookup"><span data-stu-id="94009-115">Regions are mapped to security worlds, shown as major headings in the tables:</span></span>

<span data-ttu-id="94009-116">V produktech článkem oblast, například **Americas** karta obsahuje VÝCHOD USA, STŘED USA, ZÁPADNÍ USA veškerá mapování Americas oblasti.</span><span class="sxs-lookup"><span data-stu-id="94009-116">In the products by region article, for example, the **Americas** tab contains EAST US, CENTRAL US, WEST US all mapping to the Americas region.</span></span> 

>[!NOTE]
><span data-ttu-id="94009-117">Výjimkou je, že nám DOD VÝCHOD a nám DOD centrální mají své vlastní architektury security World.</span><span class="sxs-lookup"><span data-stu-id="94009-117">An exception is that US DOD EAST and US DOD CENTRAL have their own security worlds.</span></span> 

<span data-ttu-id="94009-118">Podobně na **Evropa** kartě, severní EVROPA a ZÁPADNÍ EVROPA obě namapovány na oblasti Evropa.</span><span class="sxs-lookup"><span data-stu-id="94009-118">Similarly, on the **Europe** tab, NORTH EUROPE and WEST EUROPE both map to the Europe region.</span></span> <span data-ttu-id="94009-119">Totéž platí také na **Asie a Tichomoří** kartě.</span><span class="sxs-lookup"><span data-stu-id="94009-119">The same is also true on the **Asia Pacific** tab.</span></span>



