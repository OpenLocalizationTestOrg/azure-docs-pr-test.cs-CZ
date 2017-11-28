---
title: "aaaReliable serializaci kolekce objektů v Azure Service Fabric | Microsoft Docs"
description: "Serializace objektu Azure Service Fabric spolehlivé kolekce"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 9d35374c-2d75-4856-b776-e59284641956
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/8/2017
ms.author: mcoskun
ms.openlocfilehash: 248defbe0ae6f65b4ac5dc7c74e80d8f1152ce94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a><span data-ttu-id="49e32-103">Spolehlivé serializace objektu kolekce v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="49e32-103">Reliable Collection object serialization in Azure Service Fabric</span></span>
<span data-ttu-id="49e32-104">Spolehlivé kolekce replikovat a zachovat jejich toomake položky se, že jsou odolné napříč selhání počítače a výpadky napájení.</span><span class="sxs-lookup"><span data-stu-id="49e32-104">Reliable Collections' replicate and persist their items toomake sure they are durable across machine failures and power outages.</span></span>
<span data-ttu-id="49e32-105">Položky tooreplicate a toopersist, tooserialize nutné spolehlivé kolekce je.</span><span class="sxs-lookup"><span data-stu-id="49e32-105">Both tooreplicate and toopersist items, Reliable Collections' need tooserialize them.</span></span>

<span data-ttu-id="49e32-106">Spolehlivé kolekce získat hello odpovídající serializátor pro daný typ z spolehlivé správce stavu.</span><span class="sxs-lookup"><span data-stu-id="49e32-106">Reliable Collections' get hello appropriate serializer for a given type from Reliable State Manager.</span></span>
<span data-ttu-id="49e32-107">Správce spolehlivé stavu obsahuje integrovanou serializátorů a umožňuje vlastní serializátorů toobe zaregistrovat pro daného typu.</span><span class="sxs-lookup"><span data-stu-id="49e32-107">Reliable State Manager contains built-in serializers and allows custom serializers toobe registered for a given type.</span></span>

## <a name="built-in-serializers"></a><span data-ttu-id="49e32-108">Předdefinované Serializátorů</span><span class="sxs-lookup"><span data-stu-id="49e32-108">Built-in Serializers</span></span>

<span data-ttu-id="49e32-109">Správce spolehlivé stavu zahrnuje integrovanou serializátor pro některé běžné typy tak, aby se Dal serializovat efektivně ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="49e32-109">Reliable State Manager includes built-in serializer for some common types, so that they can be serialized efficiently by default.</span></span> <span data-ttu-id="49e32-110">Pro ostatní typy spolehlivé správce stavu spadne zpět toouse hello [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="49e32-110">For other types, Reliable State Manager falls back toouse hello [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span></span>
<span data-ttu-id="49e32-111">Předdefinované serializátorů jsou efektivnější, protože vědí, nelze změnit jejich typy a nepotřebují tooinclude informace o typu hello jako její název typu.</span><span class="sxs-lookup"><span data-stu-id="49e32-111">Built-in serializers are more efficient since they know their types cannot change and they do not need tooinclude information about hello type like its type name.</span></span>

<span data-ttu-id="49e32-112">Správce spolehlivé stavu má integrovanou serializátor pro následující typy:</span><span class="sxs-lookup"><span data-stu-id="49e32-112">Reliable State Manager has built-in serializer for following types:</span></span> 
- <span data-ttu-id="49e32-113">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="49e32-113">Guid</span></span>
- <span data-ttu-id="49e32-114">BOOL</span><span class="sxs-lookup"><span data-stu-id="49e32-114">bool</span></span>
- <span data-ttu-id="49e32-115">Bajtů</span><span class="sxs-lookup"><span data-stu-id="49e32-115">byte</span></span>
- <span data-ttu-id="49e32-116">SByte –</span><span class="sxs-lookup"><span data-stu-id="49e32-116">sbyte</span></span>
- <span data-ttu-id="49e32-117">Byte]</span><span class="sxs-lookup"><span data-stu-id="49e32-117">byte[]</span></span>
- <span data-ttu-id="49e32-118">Char</span><span class="sxs-lookup"><span data-stu-id="49e32-118">char</span></span>
- <span data-ttu-id="49e32-119">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49e32-119">string</span></span>
- <span data-ttu-id="49e32-120">Decimal</span><span class="sxs-lookup"><span data-stu-id="49e32-120">decimal</span></span>
- <span data-ttu-id="49e32-121">Double</span><span class="sxs-lookup"><span data-stu-id="49e32-121">double</span></span>
- <span data-ttu-id="49e32-122">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="49e32-122">float</span></span>
- <span data-ttu-id="49e32-123">celá čísla</span><span class="sxs-lookup"><span data-stu-id="49e32-123">int</span></span>
- <span data-ttu-id="49e32-124">uint</span><span class="sxs-lookup"><span data-stu-id="49e32-124">uint</span></span>
- <span data-ttu-id="49e32-125">dlouhá</span><span class="sxs-lookup"><span data-stu-id="49e32-125">long</span></span>
- <span data-ttu-id="49e32-126">ulong –</span><span class="sxs-lookup"><span data-stu-id="49e32-126">ulong</span></span>
- <span data-ttu-id="49e32-127">krátký</span><span class="sxs-lookup"><span data-stu-id="49e32-127">short</span></span>
- <span data-ttu-id="49e32-128">ushort –</span><span class="sxs-lookup"><span data-stu-id="49e32-128">ushort</span></span>

## <a name="custom-serialization"></a><span data-ttu-id="49e32-129">Vlastní serializace</span><span class="sxs-lookup"><span data-stu-id="49e32-129">Custom Serialization</span></span>

<span data-ttu-id="49e32-130">Vlastní serializátorů jsou běžně používané tooincrease výkonu nebo tooencrypt hello data přes přenosu hello a na disku.</span><span class="sxs-lookup"><span data-stu-id="49e32-130">Custom serializers are commonly used tooincrease performance or tooencrypt hello data over hello wire and on disk.</span></span> <span data-ttu-id="49e32-131">Mezi z jiných důvodů jsou často efektivnější než obecné serializátor vlastní serializátorů, vzhledem k tomu nepotřebují tooserialize informace o typu hello.</span><span class="sxs-lookup"><span data-stu-id="49e32-131">Among other reasons, custom serializers are commonly more efficient than generic serializer since they don't need tooserialize information about hello type.</span></span> 

<span data-ttu-id="49e32-132">[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) je použité tooregister vlastní serializátor hello daného typu T. Tato registrace má proběhnout v hello konstrukce hello StatefulServiceBase tooensure, před zahájením obnovení, všechny spolehlivé kolekce mají přístup k toohello relevantní serializátor tooread jejich trvalá data.</span><span class="sxs-lookup"><span data-stu-id="49e32-132">[IReliableStateManager.TryAddStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) is used tooregister a custom serializer for hello given type T. This registration should happen in hello construction of hello StatefulServiceBase tooensure that before recovery starts, all Reliable Collections have access toohello relevant serializer tooread their persisted data.</span></span>

```C#
public StatefulBackendService(StatefulServiceContext context)
  : base(context)
  {
    if (!this.StateManager.TryAddStateSerializer(new OrderKeySerializer()))
    {
      throw new InvalidOperationException("Failed tooset OrderKey custom serializer");
    }
  }
```

> [!NOTE]
> <span data-ttu-id="49e32-133">Vlastní serializátorů dané priority nad předdefinované serializátorů.</span><span class="sxs-lookup"><span data-stu-id="49e32-133">Custom serializers are given precedence over built-in serializers.</span></span> <span data-ttu-id="49e32-134">Například když je zaregistrovaný vlastní serializátor int, je celá čísla použité tooserialize místo hello předdefinované serializátor pro int.</span><span class="sxs-lookup"><span data-stu-id="49e32-134">For example, when a custom serializer for int is registered, it is used tooserialize integers instead of hello built-in serializer for int.</span></span>

### <a name="how-tooimplement-a-custom-serializer"></a><span data-ttu-id="49e32-135">Jak tooimplement vlastní serializátor</span><span class="sxs-lookup"><span data-stu-id="49e32-135">How tooimplement a custom serializer</span></span>

<span data-ttu-id="49e32-136">Vlastní serializátor musí tooimplement hello [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="49e32-136">A custom serializer needs tooimplement hello [IStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) interface.</span></span>

> [!NOTE]
> <span data-ttu-id="49e32-137">IStateSerializer<T> zahrnuje přetížení pro zápis a čtení, která přebírá další t označuje základní hodnota.</span><span class="sxs-lookup"><span data-stu-id="49e32-137">IStateSerializer<T> includes an overload for Write and Read that takes in an additional T called base value.</span></span> <span data-ttu-id="49e32-138">Toto rozhraní API je pro rozdílové serializace.</span><span class="sxs-lookup"><span data-stu-id="49e32-138">This API is for differential serialization.</span></span> <span data-ttu-id="49e32-139">Aktuálně nebude vystavena, funkce rozdílové serializace.</span><span class="sxs-lookup"><span data-stu-id="49e32-139">Currently differential serialization feature is not exposed.</span></span> <span data-ttu-id="49e32-140">Proto nejsou tyto dvě přetížení volá dokud rozdílové serializace je vystavený a povolený.</span><span class="sxs-lookup"><span data-stu-id="49e32-140">Hence, these two overloads are not called until differential serialization is exposed and enabled.</span></span>

<span data-ttu-id="49e32-141">Následuje příklad vlastního typu, v názvem OrderKey, který obsahuje čtyři vlastnosti</span><span class="sxs-lookup"><span data-stu-id="49e32-141">Following is an example custom type called OrderKey that contains four properties</span></span>

```C#
public class OrderKey : IComparable<OrderKey>, IEquatable<OrderKey>
{
    public byte Warehouse { get; set; }

    public short District { get; set; }

    public int Customer { get; set; }

    public long Order { get; set; }

    #region Object Overrides for GetHashCode, CompareTo and Equals
    #endregion
}
```

<span data-ttu-id="49e32-142">Následuje příklad implementace IStateSerializer<OrderKey>.</span><span class="sxs-lookup"><span data-stu-id="49e32-142">Following is an example implementation of IStateSerializer<OrderKey>.</span></span>
<span data-ttu-id="49e32-143">Všimněte si, že čtení a zápis přetížení, které provést ve baseValue, jejich příslušné přetížení volání pro předávání kompatibility.</span><span class="sxs-lookup"><span data-stu-id="49e32-143">Note that Read and Write overloads that take in baseValue, call their respective overload for forwards compatibility.</span></span>

```C#
public class OrderKeySerializer : IStateSerializer<OrderKey>
{
  OrderKey IStateSerializer<OrderKey>.Read(BinaryReader reader)
  {
      var value = new OrderKey();
      value.Warehouse = reader.ReadByte();
      value.District = reader.ReadInt16();
      value.Customer = reader.ReadInt32();
      value.Order = reader.ReadInt64();

      return value;
  }

  void IStateSerializer<OrderKey>.Write(OrderKey value, BinaryWriter writer)
  {
      writer.Write(value.Warehouse);
      writer.Write(value.District);
      writer.Write(value.Customer);
      writer.Write(value.Order);
  }
  
  // Read overload for differential de-serialization
  OrderKey IStateSerializer<OrderKey>.Read(OrderKey baseValue, BinaryReader reader)
  {
      return ((IStateSerializer<OrderKey>)this).Read(reader);
  }

  // Write overload for differential serialization
  void IStateSerializer<OrderKey>.Write(OrderKey baseValue, OrderKey newValue, BinaryWriter writer)
  {
      ((IStateSerializer<OrderKey>)this).Write(newValue, writer);
  }
}
```

## <a name="upgradability"></a><span data-ttu-id="49e32-144">Zdokonalovány</span><span class="sxs-lookup"><span data-stu-id="49e32-144">Upgradability</span></span>
<span data-ttu-id="49e32-145">V [vrácení upgradu aplikace](service-fabric-application-upgrade.md), hello upgrade je použité tooa dílčí sadu uzlů, jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="49e32-145">In a [rolling application upgrade](service-fabric-application-upgrade.md), hello upgrade is applied tooa subset of nodes, one upgrade domain at a time.</span></span> <span data-ttu-id="49e32-146">Během tohoto procesu několik domén upgradu bude v novější verzi aplikace hello a některých domén upgradu bude na hello starší verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="49e32-146">During this process, some upgrade domains will be on hello newer version of your application, and some upgrade domains will be on hello older version of your application.</span></span> <span data-ttu-id="49e32-147">Při zavedení hello hello novou verzi vaší aplikace musí být schopný tooread hello starší verzi vaše data a hello starší verzi aplikace musí být schopný tooread hello novou verzi vaše data.</span><span class="sxs-lookup"><span data-stu-id="49e32-147">During hello rollout, hello new version of your application must be able tooread hello old version of your data, and hello old version of your application must be able tooread hello new version of your data.</span></span> <span data-ttu-id="49e32-148">Pokud formát dat hello není vpřed a zpětně kompatibilní, upgrade hello může selže nebo horší, data mohou být ke ztrátě nebo poškozený.</span><span class="sxs-lookup"><span data-stu-id="49e32-148">If hello data format is not forward and backward compatible, hello upgrade may fail, or worse, data may be lost or corrupted.</span></span>

<span data-ttu-id="49e32-149">Pokud používáte integrované serializátor, není nutné tooworry o kompatibilitě.</span><span class="sxs-lookup"><span data-stu-id="49e32-149">If you are using  built-in serializer, you do not have tooworry about compatibility.</span></span>
<span data-ttu-id="49e32-150">Ale pokud používáte vlastní serializátor nebo hello DataContractSerializer, hello data mají toobe nekonečnou zpětné a předává kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="49e32-150">However, if you are using a custom serializer or hello DataContractSerializer, hello data have toobe infinitely backwards and forwards compatible.</span></span>
<span data-ttu-id="49e32-151">Jinými slovy každou verzi serializátor musí mít tooserialize toobe a deserializovat všechny verze hello typu.</span><span class="sxs-lookup"><span data-stu-id="49e32-151">In other words, each version of serializer needs toobe able tooserialize and de-serialize any version of hello type.</span></span>

<span data-ttu-id="49e32-152">Uživatelé kontraktu dat postupujte podle pravidla hello dobře definovaný verzí pro přidání, odebrání a změna pole.</span><span class="sxs-lookup"><span data-stu-id="49e32-152">Data Contract users should follow hello well-defined versioning rules for adding, removing, and changing fields.</span></span> <span data-ttu-id="49e32-153">Kontrakt dat má také podpora zabývají neznámé pole, zapojování do procesu hello serializace a deserializace a plánování práce s dědičnosti tříd.</span><span class="sxs-lookup"><span data-stu-id="49e32-153">Data Contract also has support for dealing with unknown fields, hooking into hello serialization and deserialization process, and dealing with class inheritance.</span></span> <span data-ttu-id="49e32-154">Další informace najdete v tématu [pomocí kontrakt dat](https://msdn.microsoft.com/library/ms733127.aspx).</span><span class="sxs-lookup"><span data-stu-id="49e32-154">For more information, see [Using Data Contract](https://msdn.microsoft.com/library/ms733127.aspx).</span></span>

<span data-ttu-id="49e32-155">Vlastní serializátor uživatelé by měl splňovat toohello pokynů hello serializátor používají toomake, zda je zpětné a předává kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="49e32-155">Custom serializer users should adhere toohello guidelines of hello serializer they are using toomake sure it is backwards and forwards compatible.</span></span>
<span data-ttu-id="49e32-156">Běžný způsob podpory všechny verze je přidání informací o velikosti na začátku hello a jenom přidávání volitelné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="49e32-156">Common way of supporting all versions is adding size information at hello beginning and only adding optional properties.</span></span>
<span data-ttu-id="49e32-157">Tímto způsobem jednotlivých verzí může číst co nejvíc může a přejít přes hello zbývající část datového proudu hello.</span><span class="sxs-lookup"><span data-stu-id="49e32-157">This way each version can read as much it can and jump over hello remaining part of hello stream.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49e32-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49e32-158">Next steps</span></span>
  * [<span data-ttu-id="49e32-159">Serializace a upgrade</span><span class="sxs-lookup"><span data-stu-id="49e32-159">Serialization and upgrade</span></span>](service-fabric-application-upgrade-data-serialization.md)
  * [<span data-ttu-id="49e32-160">Referenční informace pro vývojáře pro spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="49e32-160">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * <span data-ttu-id="49e32-161">[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49e32-161">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>
  * <span data-ttu-id="49e32-162">[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49e32-162">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>
  * <span data-ttu-id="49e32-163">Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="49e32-163">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>
  * <span data-ttu-id="49e32-164">Zjistěte, jak toouse pokročilé funkce při upgradu vaší aplikace tím, že odkazuje příliš[Pokročilá témata](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="49e32-164">Learn how toouse advanced functionality while upgrading your application by referring too[Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>
  * <span data-ttu-id="49e32-165">Řešení běžných potíží v upgradů aplikací tím, že odkazuje toohello kroky v [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="49e32-165">Fix common problems in application upgrades by referring toohello steps in [Troubleshooting Application Upgrades](service-fabric-application-upgrade-troubleshooting.md).</span></span>
