Aplikace .NET mohou použít hello **StackExchange.Redis** mezipaměti klienta, které lze konfigurovat v sadě Visual Studio pomocí balíčku NuGet, který zjednodušuje konfiguraci aplikací klientů mezipaměti hello. 

> [!NOTE]
> Další informace najdete v tématu hello [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) githubu stránku a hello [dokumentaci ke klientu mezipaměti StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis#documentation).
> 
> 

tooconfigure klientskou aplikaci v sadě Visual Studio pomocí balíčku StackExchange.Redis NuGet, hello klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a zvolte **spravovat balíčky NuGet**. 

![Správa balíčků NuGet](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Typ **StackExchange.Redis** nebo **StackExchange.Redis.StrongName** do textového pole hledání text hello, vyberte požadovanou verzi hello hello výsledky a klikněte na **nainstalovat**.

> [!NOTE]
> Pokud dáváte přednost toouse silným názvem verzi hello **StackExchange.Redis** klientské knihovny, zvolte **StackExchange.Redis.StrongName**; jinak vyberte **StackExchange.Redis**.
> 
> 

![Balíček StackExchange.Redis NuGet](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

Hello NuGet balíček stáhne a přidá hello požadované odkazy na sestavení pro vašeho klienta aplikace tooaccess Azure Redis Cache pomocí klienta mezipaměti StackExchange.Redis hello.

> [!NOTE]
> Pokud jste dříve nakonfigurovali vašeho projektu toouse StackExchange.Redis, můžete zkontrolovat pro balíček aktualizace toohello z hello **Správce balíčků NuGet**. toocheck pro a nainstalujte aktualizované verze hello balíčku StackExchange.Redis NuGet, klikněte na tlačítko **aktualizace** v hello hello **Správce balíčků NuGet** okno. Pokud aktualizaci toohello balíčku StackExchange.Redis NuGet je k dispozici, můžete je aktualizovat vaše projektu toouse hello aktualizované verze.
> 
> 

Můžete taky nainstalovat hello balíčku StackExchange.Redis NuGet kliknutím **Správce balíčků NuGet**, **Konzola správce balíčků** z hello **nástroje** nabídce a spuštěné hello Následující příkaz z hello **Konzola správce balíčků** okno.
    
```
Install-Package StackExchange.Redis
```
