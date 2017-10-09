## <a name="what-are-service-bus-queues"></a>Co jsou fronty služby Service Bus?
Fronty služby Service Bus podporují komunikační model **zprostředkovaného zasílání zpráv**. Součásti distribuované aplikace při používání front nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím fronty, která slouží jako zprostředkovatel. Autor zprávy (odesílatel) předá zprávu fronty toohello a poté pokračuje v jejím zpracování. Spotřebitel zprávy (příjemce) asynchronně, vrátí uvítací zprávu z fronty hello a zpracovává je. Hello producent neobsahovala toowait na odpověď od příjemce hello v pořadí toocontinue tooprocess a odesláním dalších zpráv. Fronty nabízejí **First In, první Out (FIFO)** zprávy tooone doručení nebo další konkurenčních spotřebitelů. To znamená jsou zprávy obvykle přijme a zpracuje hello příjemci v hello pořadí, ve kterém byly přidány toohello fronty, a každou zprávu přijme a zpracuje jenom jeden příjemce zprávy.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Fronty služby Service Bus představují univerzální technologii, kterou můžete použít pro celou řadu scénářů:

* Komunikace mezi webovou rolí a rolí pracovního procesu ve vícevrstvé aplikaci Azure.
* Komunikace mezi místními aplikacemi a aplikacemi hostovanými v Azure v případě hybridního řešení.
* Komunikace mezi součástmi distribuované aplikace spuštěné místně v různých organizacích nebo odděleních organizace.

Pomocí front umožňuje vám tooscale aplikace snadněji a povolit další architektura tooyour odolnost proti chybám.


