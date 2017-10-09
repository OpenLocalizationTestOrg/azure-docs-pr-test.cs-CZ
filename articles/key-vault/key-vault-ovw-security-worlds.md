---
ms.assetid: 
title: architektury security World aaaAzure Key Vault | Microsoft Docs
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 1995119c9e9886829d6c50af921f275d10e382f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a><span data-ttu-id="93bce-102">Azure Key Vault architektury security World a geografické hranice.</span><span class="sxs-lookup"><span data-stu-id="93bce-102">Azure Key Vault security worlds and geographic boundaries</span></span>

<span data-ttu-id="93bce-103">Azure Key Vault je víceklientské služby a používá fond modulů hardwarového zabezpečení (HSM) v každém umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="93bce-103">Azure Key Vault is a multi-tenant service and uses a pool of Hardware Security Modules (HSMs) in each Azure location.</span></span> 

<span data-ttu-id="93bce-104">Všechny moduly hardwarového zabezpečení v Azure umístěních v hello hello se stejným zeměpisnou oblast sdílejí stejnou hranici kryptografických (Thales Security World).</span><span class="sxs-lookup"><span data-stu-id="93bce-104">All HSMs at Azure locations in hello same geographic region share hello same cryptographic boundary (Thales Security World).</span></span> <span data-ttu-id="93bce-105">Východ USA a západ USA sdílet hello stejné architektury security world, protože náleží toohello USA geografické umístění.</span><span class="sxs-lookup"><span data-stu-id="93bce-105">For example, East US and West US share hello same security world because they belong toohello US geo location.</span></span> <span data-ttu-id="93bce-106">Podobně všech Azure umístění ve sdílené složce Japonsko hello stejné architektury security world a všechny Azure umístění v Austrálii, Indie a tak dále.</span><span class="sxs-lookup"><span data-stu-id="93bce-106">Similarly, all Azure locations in Japan share hello same security world and all Azure locations in Australia, India, and so on.</span></span> 

## <a name="backup-and-restore-behavior"></a><span data-ttu-id="93bce-107">Zálohování a obnovení chování</span><span class="sxs-lookup"><span data-stu-id="93bce-107">Backup and restore behavior</span></span>

<span data-ttu-id="93bce-108">Zálohy klíče z trezoru klíčů v jednom umístění Azure může být obnovena tooa trezoru klíčů v jiném umístění Azure, tak dlouho, dokud jsou splněny obě tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="93bce-108">A backup taken of a key from a key vault in one Azure location can be restored tooa key vault in another Azure location, as long as both of these conditions are true:</span></span>

- <span data-ttu-id="93bce-109">Obě hello Azure umístění patří toohello stejného geografického umístění</span><span class="sxs-lookup"><span data-stu-id="93bce-109">Both of hello Azure locations belong toohello same geographical location</span></span>
- <span data-ttu-id="93bce-110">Obě trezorů klíčů hello patří toohello stejného předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="93bce-110">Both of hello key vaults belong toohello same Azure subscription</span></span>

<span data-ttu-id="93bce-111">Například může být zálohy podle konkrétního předplatného klíče v trezoru klíčů v Západní Indie pouze obnovené tooanother trezoru klíčů v hello stejného předplatného a geografická umístění; Indie – Západ, Indie centrální nebo – Jih, Indie.</span><span class="sxs-lookup"><span data-stu-id="93bce-111">For example, a backup taken by a given subscription of a key in a key vault in West India, can only be restored tooanother key vault in hello same subscription and geo location; West India, Central India or South India.</span></span>

## <a name="regions-and-products"></a><span data-ttu-id="93bce-112">Oblasti a produkty</span><span class="sxs-lookup"><span data-stu-id="93bce-112">Regions and products</span></span>

- [<span data-ttu-id="93bce-113">Oblasti Azure</span><span class="sxs-lookup"><span data-stu-id="93bce-113">Azure regions</span></span>](https://azure.microsoft.com/regions/)
- [<span data-ttu-id="93bce-114">Produkty společnosti Microsoft podle oblasti</span><span class="sxs-lookup"><span data-stu-id="93bce-114">Microsoft products by region</span></span>](https://azure.microsoft.com/regions/services/)

<span data-ttu-id="93bce-115">Oblasti jsou namapované toosecurity světů, zobrazené jako hlavní záhlaví v tabulkách hello:</span><span class="sxs-lookup"><span data-stu-id="93bce-115">Regions are mapped toosecurity worlds, shown as major headings in hello tables:</span></span>

<span data-ttu-id="93bce-116">V produktech hello článkem oblast, například hello **Americas** karta obsahuje VÝCHOD USA, STŘED USA, ZÁPADNÍ USA všechny oblasti Americas toohello mapování.</span><span class="sxs-lookup"><span data-stu-id="93bce-116">In hello products by region article, for example, hello **Americas** tab contains EAST US, CENTRAL US, WEST US all mapping toohello Americas region.</span></span> 

>[!NOTE]
><span data-ttu-id="93bce-117">Výjimkou je, že nám DOD VÝCHOD a nám DOD centrální mají své vlastní architektury security World.</span><span class="sxs-lookup"><span data-stu-id="93bce-117">An exception is that US DOD EAST and US DOD CENTRAL have their own security worlds.</span></span> 

<span data-ttu-id="93bce-118">Podobně na hello **Evropa** kartě, severní EVROPA a ZÁPADNÍ EVROPA obě namapovány toohello Evropa oblast.</span><span class="sxs-lookup"><span data-stu-id="93bce-118">Similarly, on hello **Europe** tab, NORTH EUROPE and WEST EUROPE both map toohello Europe region.</span></span> <span data-ttu-id="93bce-119">Hello totéž platí také na hello **Asie a Tichomoří** kartě.</span><span class="sxs-lookup"><span data-stu-id="93bce-119">hello same is also true on hello **Asia Pacific** tab.</span></span>



