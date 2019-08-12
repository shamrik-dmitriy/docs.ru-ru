---
title: Устойчивость и высокий уровень доступности в микрослужбах
description: Микрослужбы должны разрабатываться таким образом, чтобы выдерживать временные сбои сети и зависимостей и сохранять устойчивость для достижения высокого уровня доступности.
ms.date: 09/20/2018
ms.openlocfilehash: bb1bef0c9cc08e43aed80a29effe89587fb296f6
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68675061"
---
# <a name="resiliency-and-high-availability-in-microservices"></a><span data-ttu-id="311fa-103">Устойчивость и высокий уровень доступности в микрослужбах</span><span class="sxs-lookup"><span data-stu-id="311fa-103">Resiliency and high availability in microservices</span></span>

<span data-ttu-id="311fa-104">Непредвиденные сбои являются одной из самых сложных для разрешения проблем, особенно в распределенной системе.</span><span class="sxs-lookup"><span data-stu-id="311fa-104">Dealing with unexpected failures is one of the hardest problems to solve, especially in a distributed system.</span></span> <span data-ttu-id="311fa-105">При написании кода разработчики уделяют особое внимание обработке исключений, а при тестировании именно этот аспект отнимает больше всего времени.</span><span class="sxs-lookup"><span data-stu-id="311fa-105">Much of the code that developers write involves handling exceptions, and this is also where the most time is spent in testing.</span></span> <span data-ttu-id="311fa-106">Проблема здесь не просто в написании кода для обработки сбоев.</span><span class="sxs-lookup"><span data-stu-id="311fa-106">The problem is more involved than writing code to handle failures.</span></span> <span data-ttu-id="311fa-107">Что происходит при сбое на компьютере, где работает микрослужба?</span><span class="sxs-lookup"><span data-stu-id="311fa-107">What happens when the machine where the microservice is running fails?</span></span> <span data-ttu-id="311fa-108">Необходимо не просто определить сбой микрослужбы (что само по себе непросто), но и найти способ перезапустить микрослужбу.</span><span class="sxs-lookup"><span data-stu-id="311fa-108">Not only do you need to detect this microservice failure (a hard problem on its own), but you also need something to restart your microservice.</span></span>

<span data-ttu-id="311fa-109">Микрослужба должна быть устойчива к сбоям и часто перезапускаться на другом компьютере без ущерба для доступности.</span><span class="sxs-lookup"><span data-stu-id="311fa-109">A microservice needs to be resilient to failures and to be able to restart often on another machine for availability.</span></span> <span data-ttu-id="311fa-110">Устойчивость также относится к сохранению состояния микрослужбы и возможности восстановить это состояние и успешно перезапуститься.</span><span class="sxs-lookup"><span data-stu-id="311fa-110">This resiliency also comes down to the state that was saved on behalf of the microservice, where the microservice can recover this state from, and whether the microservice can restart successfully.</span></span> <span data-ttu-id="311fa-111">Другими словами, необходима устойчивость функции вычисления (процесс можно перезапустить в любое время), а также устойчивость состояния или данных (без потери данных, с сохранением согласованности данных).</span><span class="sxs-lookup"><span data-stu-id="311fa-111">In other words, there needs to be resiliency in the compute capability (the process can restart at any time) as well as resilience in the state or data (no data loss, and the data remains consistent).</span></span>

<span data-ttu-id="311fa-112">Проблемы устойчивости актуальны и для других ситуаций, например, если сбой происходит во время обновления приложения.</span><span class="sxs-lookup"><span data-stu-id="311fa-112">The problems of resiliency are compounded during other scenarios, such as when failures occur during an application upgrade.</span></span> <span data-ttu-id="311fa-113">Микрослужба, работающая с системой развертывания, должна определить, может ли она перейти к новой версии или лучше откатиться до предыдущей версии для сохранения состояния.</span><span class="sxs-lookup"><span data-stu-id="311fa-113">The microservice, working with the deployment system, needs to determine whether it can continue to move forward to the newer version or instead roll back to a previous version to maintain a consistent state.</span></span> <span data-ttu-id="311fa-114">Необходимо подумать, достаточно ли доступных компьютеров для обновления или как восстановить предыдущие версии микрослужбы.</span><span class="sxs-lookup"><span data-stu-id="311fa-114">Questions such as whether enough machines are available to keep moving forward and how to recover previous versions of the microservice need to be considered.</span></span> <span data-ttu-id="311fa-115">Микрослужба должна передавать сведения о своей работоспособности, чтобы все приложение и оркестратор могли принимать эти решения.</span><span class="sxs-lookup"><span data-stu-id="311fa-115">This requires the microservice to emit health information so that the overall application and orchestrator can make these decisions.</span></span>

<span data-ttu-id="311fa-116">Кроме того,устойчивость связана с поведением облачных систем.</span><span class="sxs-lookup"><span data-stu-id="311fa-116">In addition, resiliency is related to how cloud-based systems must behave.</span></span> <span data-ttu-id="311fa-117">Как уже упоминалось, облачная система должна принимать сбои и пытаться восстановиться автоматически.</span><span class="sxs-lookup"><span data-stu-id="311fa-117">As mentioned, a cloud-based system must embrace failures and must try to automatically recover from them.</span></span> <span data-ttu-id="311fa-118">Например, в случае сбоя сети или контейнера клиентские приложения или клиентские службы должны иметь стратегию для повторной отправки сообщений или запросов, поскольку, как правило, сбои в облаке являются частичными.</span><span class="sxs-lookup"><span data-stu-id="311fa-118">For instance, in case of network or container failures, client apps or client services must have a strategy to retry sending messages or to retry requests, since in many cases failures in the cloud are partial.</span></span> <span data-ttu-id="311fa-119">В этом руководстве в разделе [Реализации устойчивых приложений](../implement-resilient-applications/index.md) рассказывается, как обрабатывать частичные сбои.</span><span class="sxs-lookup"><span data-stu-id="311fa-119">The [Implementing Resilient Applications](../implement-resilient-applications/index.md) section in this guide addresses how to handle partial failure.</span></span> <span data-ttu-id="311fa-120">Там описываются такие методы, как повторная попытка с экспоненциальной задержкой или шаблон размыкателя цепи в .NET Core с использованием библиотек, например [Polly](https://github.com/App-vNext/Polly), где содержится целый ряд политик для обработки таких сбоев.</span><span class="sxs-lookup"><span data-stu-id="311fa-120">It describes techniques like retries with exponential backoff or the Circuit Breaker pattern in .NET Core by using libraries like [Polly](https://github.com/App-vNext/Polly), which offers a large variety of policies to handle this subject.</span></span>

## <a name="health-management-and-diagnostics-in-microservices"></a><span data-ttu-id="311fa-121">Управление работоспособностью и диагностика микрослужб</span><span class="sxs-lookup"><span data-stu-id="311fa-121">Health management and diagnostics in microservices</span></span>

<span data-ttu-id="311fa-122">Микрослужба должна сообщать о своей работоспособности и диагностике. Это может казаться очевидным, и об этом часто забывают.</span><span class="sxs-lookup"><span data-stu-id="311fa-122">It may seem obvious, and it's often overlooked, but a microservice must report its health and diagnostics.</span></span> <span data-ttu-id="311fa-123">Но, в противном случае, сведений о работе будет недостаточно.</span><span class="sxs-lookup"><span data-stu-id="311fa-123">Otherwise, there's little insight from an operations perspective.</span></span> <span data-ttu-id="311fa-124">Сопоставлять диагностические события в наборе независимых служб и решать проблемы расфазировки сигналов, чтобы разобраться в порядке событий, довольно непросто.</span><span class="sxs-lookup"><span data-stu-id="311fa-124">Correlating diagnostic events across a set of independent services and dealing with machine clock skews to make sense of the event order is challenging.</span></span> <span data-ttu-id="311fa-125">Так же, как вы взаимодействуете с микрослужбами по согласованным протоколам и форматам данных, необходимо стандартизировать запись событий работоспособности и диагностических событий, которые сохраняются в хранилище событий, доступном для запросов и просмотра.</span><span class="sxs-lookup"><span data-stu-id="311fa-125">In the same way that you interact with a microservice over agreed-upon protocols and data formats, there's a need for standardization in how to log health and diagnostic events that ultimately end up in an event store for querying and viewing.</span></span> <span data-ttu-id="311fa-126">При использовании микрослужб важно, чтобы команды разработчиков согласовали единый формат ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="311fa-126">In a microservices approach, it's key that different teams agree on a single logging format.</span></span> <span data-ttu-id="311fa-127">Необходим единообразный подход к просмотру диагностических событий в приложении.</span><span class="sxs-lookup"><span data-stu-id="311fa-127">There needs to be a consistent approach to viewing diagnostic events in the application.</span></span>

### <a name="health-checks"></a><span data-ttu-id="311fa-128">Проверки работоспособности</span><span class="sxs-lookup"><span data-stu-id="311fa-128">Health checks</span></span>

<span data-ttu-id="311fa-129">Работоспособность отличается от диагностики.</span><span class="sxs-lookup"><span data-stu-id="311fa-129">Health is different from diagnostics.</span></span> <span data-ttu-id="311fa-130">Работоспособность связана с отчетами микрослужбы о своем текущем состоянии для принятия необходимых мер.</span><span class="sxs-lookup"><span data-stu-id="311fa-130">Health is about the microservice reporting its current state to take appropriate actions.</span></span> <span data-ttu-id="311fa-131">Хороший пример — работа с механизмами обновления и развертывания для поддержания доступности.</span><span class="sxs-lookup"><span data-stu-id="311fa-131">A good example is working with upgrade and deployment mechanisms to maintain availability.</span></span> <span data-ttu-id="311fa-132">И хотя служба может утратить работоспособность в результате аварийного завершения процесса или перезагрузки компьютера, она по-прежнему может сохранять свою функциональность.</span><span class="sxs-lookup"><span data-stu-id="311fa-132">Although a service might currently be unhealthy due to a process crash or machine reboot, the service might still be operational.</span></span> <span data-ttu-id="311fa-133">Вы только усугубите ситуацию, если выполните обновление.</span><span class="sxs-lookup"><span data-stu-id="311fa-133">The last thing you need is to make this worse by performing an upgrade.</span></span> <span data-ttu-id="311fa-134">Рекомендуется сначала провести расследование и дать микрослужбе время на восстановление.</span><span class="sxs-lookup"><span data-stu-id="311fa-134">The best approach is to do an investigation first or allow time for the microservice to recover.</span></span> <span data-ttu-id="311fa-135">События работоспособности от микрослужбы помогают принимать обоснованные решения и, по сути, создавать самовосстанавливающиеся службы.</span><span class="sxs-lookup"><span data-stu-id="311fa-135">Health events from a microservice help us make informed decisions and, in effect, help create self-healing services.</span></span>

<span data-ttu-id="311fa-136">В разделе [Выполнение проверок работоспособности в службах ASP.NET Core](../implement-resilient-applications/monitor-app-health.md#implement-health-checks-in-aspnet-core-services) объясняется, как использовать библиотеку ASP.NET HealthChecks в микрослужбах, чтобы они сообщали о своем состоянии службе мониторинга и можно было принять необходимые меры.</span><span class="sxs-lookup"><span data-stu-id="311fa-136">In the [Implementing health checks in ASP.NET Core services](../implement-resilient-applications/monitor-app-health.md#implement-health-checks-in-aspnet-core-services) section of this guide, we explain how to use a new ASP.NET HealthChecks library in your microservices so they can report their state to a monitoring service to take appropriate actions.</span></span>

<span data-ttu-id="311fa-137">Кроме того, вы можете использовать отличную библиотеку открытого исходного кода под названием Beat Pulse, доступную на [GitHub](https://github.com/Xabaril/BeatPulse) и в качестве [пакета NuGet](https://www.nuget.org/packages/BeatPulse/).</span><span class="sxs-lookup"><span data-stu-id="311fa-137">You also have the option of using an excellent open-source library called Beat Pulse, available on [GitHub](https://github.com/Xabaril/BeatPulse) and as a [NuGet package](https://www.nuget.org/packages/BeatPulse/).</span></span> <span data-ttu-id="311fa-138">Эта библиотека также выполняет проверки работоспособности и обрабатывает два вида проверок:</span><span class="sxs-lookup"><span data-stu-id="311fa-138">This library also does health checks, with a twist, it handles two types of checks:</span></span>

- <span data-ttu-id="311fa-139">**Жизнеспособность**. Проверяет, что микрослужбы находятся в активном состоянии, то есть могут принимать запросы и отвечать.</span><span class="sxs-lookup"><span data-stu-id="311fa-139">**Liveness**: Checks if the microservice is alive, that is, if it’s able to accept requests and respond.</span></span> 
- <span data-ttu-id="311fa-140">**Готовность**. Проверяет готовность зависимостей микрослужбы (база данных, службы очередей, т. д.), чтобы микрослужба выполняла поставленные задачи.</span><span class="sxs-lookup"><span data-stu-id="311fa-140">**Readiness**: Checks if the microservice's dependencies (Database, queue services, etc.) are themselves ready, so the microservice can do what it’s supposed to do.</span></span> 

### <a name="using-diagnostics-and-logs-event-streams"></a><span data-ttu-id="311fa-141">Использование потоков диагностики и записи событий</span><span class="sxs-lookup"><span data-stu-id="311fa-141">Using diagnostics and logs event streams</span></span>

<span data-ttu-id="311fa-142">Журналы содержат сведения о работе приложения или службы, в том числе исключения, предупреждения и информационные сообщения.</span><span class="sxs-lookup"><span data-stu-id="311fa-142">Logs provide information about how an application or service is running, including exceptions, warnings, and simple informational messages.</span></span> <span data-ttu-id="311fa-143">Как правило, журналы имеют текстовый формат, где каждая строка обозначает событие, хотя исключения часто отображаются в виде трассировки на нескольких строках.</span><span class="sxs-lookup"><span data-stu-id="311fa-143">Usually, each log is in a text format with one line per event, although exceptions also often show the stack trace across multiple lines.</span></span>

<span data-ttu-id="311fa-144">В монолитных серверных приложениях можно просто записывать журналы в файл на диске (файл журнала), а затем анализировать его с помощью любого инструмента.</span><span class="sxs-lookup"><span data-stu-id="311fa-144">In monolithic server-based applications, you can simply write logs to a file on disk (a logfile) and then analyze it with any tool.</span></span> <span data-ttu-id="311fa-145">Поскольку приложение выполняется на фиксированном сервере или виртуальной машине, анализировать поток событий обычно не слишком сложно.</span><span class="sxs-lookup"><span data-stu-id="311fa-145">Since application execution is limited to a fixed server or VM, it generally isn't too complex to analyze the flow of events.</span></span> <span data-ttu-id="311fa-146">Но в распределенных приложениях, где несколько служб выполняется на многих узлах в кластере оркестратора, сопоставить распределенные события бывает непросто.</span><span class="sxs-lookup"><span data-stu-id="311fa-146">However, in a distributed application where multiple services are executed across many nodes in an orchestrator cluster, being able to correlate distributed events is a challenge.</span></span>

<span data-ttu-id="311fa-147">Приложение на основе микрослужб не должно пытаться самостоятельно сохранить поток вывода событий или файлов журнала или хотя бы управлять маршрутизацией событий в центральное хранилище.</span><span class="sxs-lookup"><span data-stu-id="311fa-147">A microservice-based application should not try to store the output stream of events or logfiles by itself, and not even try to manage the routing of the events to a central place.</span></span> <span data-ttu-id="311fa-148">Оно должно быть прозрачным, то есть каждый процесс должен просто записывать поток событий в стандартный поток вывода, который будет поступать в инфраструктуру среды выполнения, где оно запущено.</span><span class="sxs-lookup"><span data-stu-id="311fa-148">It should be transparent, meaning that each process should just write its event stream to a standard output that underneath will be collected by the execution environment infrastructure where it's running.</span></span> <span data-ttu-id="311fa-149">Пример маршрутизатора потока событий — [Microsoft.Diagnostic.EventFlow](https://github.com/Azure/diagnostics-eventflow), который собирает потоки событий из нескольких источников и публикует их в систему вывода.</span><span class="sxs-lookup"><span data-stu-id="311fa-149">An example of these event stream routers is [Microsoft.Diagnostic.EventFlow](https://github.com/Azure/diagnostics-eventflow), which collects event streams from multiple sources and publishes it to output systems.</span></span> <span data-ttu-id="311fa-150">Это может быть стандартный поток вывода для среды разработки или облачных систем, таких как [Azure Monitor](https://azure.microsoft.com/services/monitor//) и [Диагностика Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview).</span><span class="sxs-lookup"><span data-stu-id="311fa-150">These can include simple standard output for a development environment or cloud systems like [Azure Monitor](https://azure.microsoft.com/services/monitor//) and [Azure Diagnostics](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview).</span></span> <span data-ttu-id="311fa-151">Существуют надежные сторонние платформы и инструменты анализа журналов, которые могут выполнять поиск, выводить предупреждения, составлять отчеты и отслеживать журналы, даже в режиме реального времени, например [Splunk](https://www.splunk.com/goto/Splunk_Log_Management?ac=ga_usa_log_analysis_phrase_Mar17&_kk=logs%20analysis&gclid=CNzkzIrex9MCFYGHfgodW5YOtA).</span><span class="sxs-lookup"><span data-stu-id="311fa-151">There are also good third-party log analysis platforms and tools that can search, alert, report, and monitor logs, even in real time, like [Splunk](https://www.splunk.com/goto/Splunk_Log_Management?ac=ga_usa_log_analysis_phrase_Mar17&_kk=logs%20analysis&gclid=CNzkzIrex9MCFYGHfgodW5YOtA).</span></span>

### <a name="orchestrators-managing-health-and-diagnostics-information"></a><span data-ttu-id="311fa-152">Оркестраторы, управляющие сведениями о работоспособности и диагностике</span><span class="sxs-lookup"><span data-stu-id="311fa-152">Orchestrators managing health and diagnostics information</span></span>

<span data-ttu-id="311fa-153">При создании приложения на основе микрослужб вы имеете дело со сложной структурой.</span><span class="sxs-lookup"><span data-stu-id="311fa-153">When you create a microservice-based application, you need to deal with complexity.</span></span> <span data-ttu-id="311fa-154">Разумеется, с одной микрослужбой работать просто, но все усложняется при наличии десятков и сотен типов и тысяч экземпляров микрослужб.</span><span class="sxs-lookup"><span data-stu-id="311fa-154">Of course, a single microservice is simple to deal with, but dozens or hundreds of types and thousands of instances of microservices is a complex problem.</span></span> <span data-ttu-id="311fa-155">Недостаточно просто создать архитектуру микрослужб — необходимо обеспечить высокую доступность, адресуемость, устойчивость, работоспособность и диагностику, если вы хотите получить стабильную и слаженную систему.</span><span class="sxs-lookup"><span data-stu-id="311fa-155">It isn't just about building your microservice architecture—you also need high availability, addressability, resiliency, health, and diagnostics if you intend to have a stable and cohesive system.</span></span>

![Оркестраторы предоставляют вспомогательную платформу для запуска микрослужб.](./media/image22.png)

<span data-ttu-id="311fa-157">**Рис. 4-22**.</span><span class="sxs-lookup"><span data-stu-id="311fa-157">**Figure 4-22**.</span></span> <span data-ttu-id="311fa-158">Платформа микрослужб необходима для управления работоспособностью приложения</span><span class="sxs-lookup"><span data-stu-id="311fa-158">A Microservice Platform is fundamental for an application's health management</span></span>

<span data-ttu-id="311fa-159">Сложные проблемы, показанные на рисунке 4-22, очень трудно решить самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="311fa-159">The complex problems shown in Figure 4-22 are very hard to solve by yourself.</span></span> <span data-ttu-id="311fa-160">Командам разработчиков следует сосредоточиться на решении бизнес-задач и разработке пользовательских приложений на основе микрослужб.</span><span class="sxs-lookup"><span data-stu-id="311fa-160">Development teams should focus on solving business problems and building custom applications with microservice-based approaches.</span></span> <span data-ttu-id="311fa-161">Им не следует думать о решении сложных проблем с инфраструктурой. В противном случае стоимость приложения на базе микрослужб была бы огромной.</span><span class="sxs-lookup"><span data-stu-id="311fa-161">They should not focus on solving complex infrastructure problems; if they did, the cost of any microservice-based application would be huge.</span></span> <span data-ttu-id="311fa-162">Поэтому и существуют платформы для микрослужб, называемые оркестраторами или кластерами микрослужб. Эти платформы решают проблемы сборки и запуска службы и эффективного использования ресурсов инфраструктуры.</span><span class="sxs-lookup"><span data-stu-id="311fa-162">Therefore, there are microservice-oriented platforms, referred to as orchestrators or microservice clusters, that try to solve the hard problems of building and running a service and using infrastructure resources efficiently.</span></span> <span data-ttu-id="311fa-163">Это упрощает создание приложений на базе микрослужб.</span><span class="sxs-lookup"><span data-stu-id="311fa-163">This reduces the complexities of building applications that use a microservices approach.</span></span>

<span data-ttu-id="311fa-164">Различные оркестраторы могут походить друг на друга, но отличаться по функциям и уровню зрелости диагностики и проверок работоспособности. Иногда это зависит от платформы ОС, как описано в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="311fa-164">Different orchestrators might sound similar, but the diagnostics and health checks offered by each of them differ in features and state of maturity, sometimes depending on the OS platform, as explained in the next section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="311fa-165">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="311fa-165">Additional resources</span></span>

- <span data-ttu-id="311fa-166">**The Twelve-Factor App. XI. Журналы. Обработка журналов как потоков событий** </span><span class="sxs-lookup"><span data-stu-id="311fa-166">**The Twelve-Factor App. XI. Logs: Treat logs as event streams** </span></span>\
  <https://12factor.net/logs>

- <span data-ttu-id="311fa-167">Репозиторий GitHub **Microsoft Diagnostic EventFlow Library**.</span><span class="sxs-lookup"><span data-stu-id="311fa-167">**Microsoft Diagnostic EventFlow Library** GitHub repo.</span></span> \
  <https://github.com/Azure/diagnostics-eventflow>

- <span data-ttu-id="311fa-168">**Система диагностики Azure** </span><span class="sxs-lookup"><span data-stu-id="311fa-168">**What is Azure Diagnostics** </span></span>\
  <https://docs.microsoft.com/azure/azure-diagnostics>

- <span data-ttu-id="311fa-169">**Connect Windows computers to the Azure Monitor service** \ (Подключение компьютеров Windows к службе Azure Monitor)</span><span class="sxs-lookup"><span data-stu-id="311fa-169">**Connect Windows computers to the Azure Monitor service** \</span></span>
  <https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows>

- <span data-ttu-id="311fa-170">**Запись значения в журнал. Использование прикладного блока семантического ведения журнала** </span><span class="sxs-lookup"><span data-stu-id="311fa-170">**Logging What You Mean: Using the Semantic Logging Application Block** </span></span>\
  <https://docs.microsoft.com/previous-versions/msp-n-p/dn440729(v=pandp.60)>

- <span data-ttu-id="311fa-171">Официальный сайт **Splunk**.</span><span class="sxs-lookup"><span data-stu-id="311fa-171">**Splunk** Official site.</span></span> \
  <https://www.splunk.com/>

- <span data-ttu-id="311fa-172">**EventSource Class** Интерфейс API для трассировки событий Windows (ETW) </span><span class="sxs-lookup"><span data-stu-id="311fa-172">**EventSource Class** API for events tracing for Windows (ETW) </span></span>\
  [https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource](xref:System.Diagnostics.Tracing.EventSource)

>[!div class="step-by-step"]
><span data-ttu-id="311fa-173">[Назад](microservice-based-composite-ui-shape-layout.md)
>[Вперед](scalable-available-multi-container-microservice-applications.md)</span><span class="sxs-lookup"><span data-stu-id="311fa-173">[Previous](microservice-based-composite-ui-shape-layout.md)
[Next](scalable-available-multi-container-microservice-applications.md)</span></span>