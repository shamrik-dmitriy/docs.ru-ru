---
title: Использование функций сопоставления шаблонов для расширения типов данных
description: Этом руководстве показано, как использовать методы сопоставления шаблонов для создания функций с помощью данных и алгоритмов, которые создаются отдельно.
ms.date: 03/13/2019
ms.custom: mvc
ms.openlocfilehash: c064af5fdf85587d0c4fa1471894122d6fe0d2f7
ms.sourcegitcommit: e994e47d3582bf09ae487ecbd53c0dac30aebaf7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58262524"
---
# <a name="tutorial-using-pattern-matching-features-to-extend-data-types"></a><span data-ttu-id="2f700-103">Учебник. Использование функций сопоставления шаблонов для расширения типов данных</span><span class="sxs-lookup"><span data-stu-id="2f700-103">Tutorial: Using pattern matching features to extend data types</span></span>

<span data-ttu-id="2f700-104">В C# 7 появились базовые функции сопоставления шаблонов.</span><span class="sxs-lookup"><span data-stu-id="2f700-104">C# 7 introduced basic pattern matching features.</span></span> <span data-ttu-id="2f700-105">Эти функции были расширены в C# 8 за счет новых выражений и шаблонов.</span><span class="sxs-lookup"><span data-stu-id="2f700-105">Those features are extended in C# 8 with new expressions and patterns.</span></span> <span data-ttu-id="2f700-106">Вы можете написать функции, которые работают так, будто вы расширили типы, которые могут быть в других библиотеках.</span><span class="sxs-lookup"><span data-stu-id="2f700-106">You can write functionality that behaves as though you extended types that may be in other libraries.</span></span> <span data-ttu-id="2f700-107">Еще один вариант использования шаблонов — создать функции, которые требуются для приложения и не являются фундаментальными функциями для расширяемого типа.</span><span class="sxs-lookup"><span data-stu-id="2f700-107">Another use for patterns is to create functionality your application requires that isn't a fundamental feature of the type being extended.</span></span>

<span data-ttu-id="2f700-108">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="2f700-108">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2f700-109">распознавать случаи, в которых следует использовать сопоставление шаблонов;</span><span class="sxs-lookup"><span data-stu-id="2f700-109">Recognize situations where pattern matching should be used.</span></span>
> * <span data-ttu-id="2f700-110">использовать выражения сопоставления шаблонов для реализации поведения с учетом типов и значений свойств;</span><span class="sxs-lookup"><span data-stu-id="2f700-110">Use pattern matching expressions to implement behavior based on types and property values.</span></span>
> * <span data-ttu-id="2f700-111">комбинировать сопоставление шаблонов и другие методы для создания полных алгоритмов.</span><span class="sxs-lookup"><span data-stu-id="2f700-111">Combine pattern matching with other techniques to create complete algorithms.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f700-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="2f700-112">Prerequisites</span></span>

<span data-ttu-id="2f700-113">Вам нужно настроить свой компьютер для работы с .NET Core, включая предварительную версию компилятора C# 8.0.</span><span class="sxs-lookup"><span data-stu-id="2f700-113">You'll need to set up your machine to run .NET Core, including the C# 8.0 preview compiler.</span></span> <span data-ttu-id="2f700-114">Предварительная версия компилятора C# 8 доступна с последней предварительной версией [Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019+preview) или [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="2f700-114">The C# 8 preview compiler is available with the latest [Visual Studio 2019 preview](https://visualstudio.microsoft.com/vs/preview/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019+preview), or the latest [.NET Core 3.0 preview](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

<span data-ttu-id="2f700-115">В этом руководстве предполагается, что вы знакомы с C# и .NET, включая Visual Studio или .NET Core CLI.</span><span class="sxs-lookup"><span data-stu-id="2f700-115">This tutorial assumes you're familiar with C# and .NET, including either Visual Studio or the .NET Core CLI.</span></span>

## <a name="scenarios-for-pattern-matching"></a><span data-ttu-id="2f700-116">Сценарии для сопоставления шаблонов</span><span class="sxs-lookup"><span data-stu-id="2f700-116">Scenarios for pattern matching</span></span>

<span data-ttu-id="2f700-117">Современная разработка часто предусматривает использование данных из нескольких источников, а также представление информации и идей на основе этих данных в одном связном приложении.</span><span class="sxs-lookup"><span data-stu-id="2f700-117">Modern development often includes integrating data from multiple sources and presenting information and insights from that data in a single cohesive application.</span></span> <span data-ttu-id="2f700-118">У вас и вашей команды не всегда будет возможность контроля над всеми типами входящих данных или доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="2f700-118">You and your team won't have control or access for all the types that represent the incoming data.</span></span>

<span data-ttu-id="2f700-119">Для классического объектно-ориентированного приложения необходимо создавать в приложении типы данных, которые представляют каждый тип данных из этих нескольких источников.</span><span class="sxs-lookup"><span data-stu-id="2f700-119">The classic object-oriented design would call for creating types in your application that represent each data type from those multiple data sources.</span></span> <span data-ttu-id="2f700-120">Затем ваше приложение будет работать с этими новыми типами, создавать иерархии наследования и виртуальные методы, а также реализовывать абстракции.</span><span class="sxs-lookup"><span data-stu-id="2f700-120">Then, your application would work with those new types, build inheritance hierarchies, create virtual methods, and implement abstractions.</span></span> <span data-ttu-id="2f700-121">Эти методы работают, и иногда это лучшее средство для решения проблемы.</span><span class="sxs-lookup"><span data-stu-id="2f700-121">Those techniques work, and sometimes they are the best tools.</span></span> <span data-ttu-id="2f700-122">Но в некоторых случаях можно писать меньше кода.</span><span class="sxs-lookup"><span data-stu-id="2f700-122">Other times you can write less code.</span></span> <span data-ttu-id="2f700-123">Вы можете писать более понятный код, используя методы, которые разделяют сами данные и операции с этими данными.</span><span class="sxs-lookup"><span data-stu-id="2f700-123">You can write more clear code using techniques that separate the data from the operations that manipulate that data.</span></span>

<span data-ttu-id="2f700-124">В этом руководстве описано, как создать и оценить приложение, которое принимает входящие данные из нескольких внешних источников для одного сценария.</span><span class="sxs-lookup"><span data-stu-id="2f700-124">In this tutorial, you'll create and explore an application that takes incoming data from several external sources for a single scenario.</span></span> <span data-ttu-id="2f700-125">Вы увидите, что **сопоставление шаблонов** позволяет эффективно использовать и обрабатывать данные такими способами, которые изначально не были частью системы.</span><span class="sxs-lookup"><span data-stu-id="2f700-125">You'll see how **pattern matching** provides an efficient way to consume and process that data in ways that weren't part of the original system.</span></span>

<span data-ttu-id="2f700-126">Рассмотрим крупную транспортную сеть, в которой для управления трафиком используются дорожные сборы и тарификация на основе пиковой загрузки.</span><span class="sxs-lookup"><span data-stu-id="2f700-126">Consider a major metro area that is using tolls and peak time pricing to manage traffic.</span></span> <span data-ttu-id="2f700-127">Вы напишете приложение, которое рассчитывает плату за автомобиль в зависимости от его типа.</span><span class="sxs-lookup"><span data-stu-id="2f700-127">You write an application that calculates tolls for a vehicle based on its type.</span></span> <span data-ttu-id="2f700-128">Затем вы добавите цены с учетом количества пассажиров в автомобиле, а также</span><span class="sxs-lookup"><span data-stu-id="2f700-128">Later enhancements incorporate pricing based on the number of occupants in the vehicle.</span></span> <span data-ttu-id="2f700-129">времени и дня недели.</span><span class="sxs-lookup"><span data-stu-id="2f700-129">Further enhancements add pricing based on the time and the day of the week.</span></span>

<span data-ttu-id="2f700-130">Из этого краткого описания вы можете быстро составить иерархию объектов для моделирования этой системы.</span><span class="sxs-lookup"><span data-stu-id="2f700-130">From that brief description, you may have quickly sketched out an object hierarchy to model this system.</span></span> <span data-ttu-id="2f700-131">Но ваши данные поступают из разных источников, включая другие системы управления регистрацией транспортных средств.</span><span class="sxs-lookup"><span data-stu-id="2f700-131">However, your data is coming from multiple sources like other vehicle registration management systems.</span></span> <span data-ttu-id="2f700-132">Эти системы предоставляют разные классы для моделирования таких данных, и у вас нет единой объектной модели, которую можно использовать.</span><span class="sxs-lookup"><span data-stu-id="2f700-132">These systems provide different classes to model that data and you don't have a single object model you can use.</span></span> <span data-ttu-id="2f700-133">При работе с этим руководством для моделирования данных автомобиля вы будете использовать упрощенные классы из этих внешних систем, как показано в следующем примере кода:</span><span class="sxs-lookup"><span data-stu-id="2f700-133">In this tutorial, you'll use these simplified classes to model for the vehicle data from these external systems, as shown in the following code:</span></span>

[!code-csharp[ExternalSystems](~/samples/csharp/tutorials/patterns/start/toll-calculator/ExternalSystems.cs)]

<span data-ttu-id="2f700-134">Скачать начальный код можно из репозитория GitHub [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/start).</span><span class="sxs-lookup"><span data-stu-id="2f700-134">You can download the starter code from the [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/start) GitHub repository.</span></span> <span data-ttu-id="2f700-135">Вы можете видеть, что классы транспортных средств принадлежат разным системам и находятся в разных пространствах имен.</span><span class="sxs-lookup"><span data-stu-id="2f700-135">You can see that the vehicle classes are from different systems, and are in different namespaces.</span></span> <span data-ttu-id="2f700-136">Вы не можете использовать другие базовые классы, кроме `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="2f700-136">No common base class, other than `System.Object` can be leveraged.</span></span>

## <a name="pattern-matching-designs"></a><span data-ttu-id="2f700-137">Схемы сопоставления шаблонов</span><span class="sxs-lookup"><span data-stu-id="2f700-137">Pattern matching designs</span></span>

<span data-ttu-id="2f700-138">Сценарий, используемый в этом руководстве, позволяет выделить виды проблем, для решения которых подходит сопоставление шаблонов.</span><span class="sxs-lookup"><span data-stu-id="2f700-138">The scenario used in this tutorial highlights the kinds of problems that pattern matching is well-suited to solve:</span></span>

- <span data-ttu-id="2f700-139">Объекты, с которыми вам нужно работать, не находятся в иерархии объектов, которая соответствует вашим целям.</span><span class="sxs-lookup"><span data-stu-id="2f700-139">The objects you need to work with aren't in an object hierarchy that matches your goals.</span></span> <span data-ttu-id="2f700-140">Вам может потребоваться работать с классами, которые являются частью несвязанных систем.</span><span class="sxs-lookup"><span data-stu-id="2f700-140">You may be working with classes that are part of unrelated systems.</span></span>
- <span data-ttu-id="2f700-141">Функции, которые вы добавляете, не является частью основной абстракции для этих классов.</span><span class="sxs-lookup"><span data-stu-id="2f700-141">The functionality you're adding isn't part of the core abstraction for these classes.</span></span> <span data-ttu-id="2f700-142">Плата за автомобиль *изменяется* в зависимости от его типа. При этом плата не является основной функцией этого автомобиля.</span><span class="sxs-lookup"><span data-stu-id="2f700-142">The toll paid by a vehicle *changes* for different types of vehicles, but the toll isn't a core function of the vehicle.</span></span>

<span data-ttu-id="2f700-143">Если *форма* данных и *операции* с данными не описаны вместе, можно использовать функцию сопоставления шаблонов в C#, чтобы упростить работу.</span><span class="sxs-lookup"><span data-stu-id="2f700-143">When the *shape* of the data and the *operations* on that data are not described together, the pattern matching features in C# make it easier to work with.</span></span>

## <a name="implement-the-basic-toll-calculations"></a><span data-ttu-id="2f700-144">Реализация расчетов базового сбора</span><span class="sxs-lookup"><span data-stu-id="2f700-144">Implement the basic toll calculations</span></span>

<span data-ttu-id="2f700-145">Самый простой расчет базового сбора выполняется с учетом типа автомобиля:</span><span class="sxs-lookup"><span data-stu-id="2f700-145">The most basic toll calculation relies only on the vehicle type:</span></span>

- <span data-ttu-id="2f700-146">`Car` — 2,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-146">A `Car` is $2.00.</span></span>
- <span data-ttu-id="2f700-147">`Taxi` — $3,50 дол. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-147">A `Taxi` is $3.50.</span></span>
- <span data-ttu-id="2f700-148">`Bus` — $5,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-148">A `Bus` is $5.00.</span></span>
- <span data-ttu-id="2f700-149">`DeliveryTruck` — $10,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-149">A `DeliveryTruck` is $10.00</span></span>

<span data-ttu-id="2f700-150">Создайте класс `TollCalculator` и реализуйте сопоставление шаблонов по типу автомобиля, чтобы получить сумму сбора.</span><span class="sxs-lookup"><span data-stu-id="2f700-150">Create a new `TollCalculator` class, and implement pattern matching on the vehicle type to get the toll amount.</span></span> <span data-ttu-id="2f700-151">В приведенном ниже примере кода показана начальная реализация класса `TollCalculator`.</span><span class="sxs-lookup"><span data-stu-id="2f700-151">The following code shows the initial implementation of the `TollCalculator`.</span></span>

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    public class TollCalculator
    {
        public decimal CalculateToll(object vehicle) =>
            vehicle switch
        {
            Car c           => 2.00m,
            Taxi t          => 3.50m,
            Bus b           => 5.00m,
            DeliveryTruck t => 10.00m,
            { }             => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
            null            => throw new ArgumentNullException(nameof(vehicle))
        };
    }
}
```

<span data-ttu-id="2f700-152">В коде предыдущего примера используется **выражение switch** (которое отличается от оператора [`switch`](../language-reference/keywords/switch.md)), проверяющее **шаблон типа**.</span><span class="sxs-lookup"><span data-stu-id="2f700-152">The preceding code uses a **switch expression** (not the same as a [`switch`](../language-reference/keywords/switch.md) statement) that tests the **type pattern**.</span></span> <span data-ttu-id="2f700-153">**Выражение switch** начинается с переменной `vehicle` в приведенном выше коде, за которой следует ключевое слово `switch`.</span><span class="sxs-lookup"><span data-stu-id="2f700-153">A **switch expression** begins with the variable, `vehicle` in the preceding code, followed by the `switch` keyword.</span></span> <span data-ttu-id="2f700-154">Далее следуют все **доступные ветви switch** внутри фигурных скобок.</span><span class="sxs-lookup"><span data-stu-id="2f700-154">Next comes all the **switch arms** inside curly braces.</span></span> <span data-ttu-id="2f700-155">Выражение `switch`вносит другие уточнения в синтаксис, который окружает оператор `switch`.</span><span class="sxs-lookup"><span data-stu-id="2f700-155">The `switch` expression makes other refinements to the syntax that surrounds the `switch` statement.</span></span> <span data-ttu-id="2f700-156">Ключевое слово `case` опущено, и результатом каждой ветви является выражение.</span><span class="sxs-lookup"><span data-stu-id="2f700-156">The `case` keyword is omitted, and the result of each arm is an expression.</span></span> <span data-ttu-id="2f700-157">Последние две ветви демонстрируют новую функцию языка.</span><span class="sxs-lookup"><span data-stu-id="2f700-157">The last two arms show a new language feature.</span></span> <span data-ttu-id="2f700-158">Случай `{ }` соответствует любому ненулевому объекту, который не совпадает с предыдущей ветвью.</span><span class="sxs-lookup"><span data-stu-id="2f700-158">The `{ }` case matches any non-null object that didn't match an earlier arm.</span></span> <span data-ttu-id="2f700-159">Этот случай позволяет перехватить все неправильные типы, переданные этому методу.</span><span class="sxs-lookup"><span data-stu-id="2f700-159">This arm catches any incorrect types passed to this method.</span></span>  <span data-ttu-id="2f700-160">Случай `{ }` должен находиться после всех случаев, соответствующих типам автомобилей,</span><span class="sxs-lookup"><span data-stu-id="2f700-160">The `{ }` case must follow the cases for each vehicle type.</span></span> <span data-ttu-id="2f700-161">иначе случай `{ }` будет иметь приоритет.</span><span class="sxs-lookup"><span data-stu-id="2f700-161">If the order were reversed, the `{ }` case would take precedence.</span></span> <span data-ttu-id="2f700-162">Наконец, шаблон со значением `null` определяет, когда этому методу передается значение `null`.</span><span class="sxs-lookup"><span data-stu-id="2f700-162">Finally, the `null` pattern detects when a `null` is passed to this method.</span></span> <span data-ttu-id="2f700-163">Шаблон `null` может быть последним, так как шаблоны другого типа соответствуют только ненулевому объекту правильного типа.</span><span class="sxs-lookup"><span data-stu-id="2f700-163">The `null` pattern can be last because the other type patterns match only a non-null object of the correct type.</span></span>

<span data-ttu-id="2f700-164">Этот код можно проверить с помощью следующего кода в файле `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="2f700-164">You can test this code using the following code in `Program.cs`:</span></span>

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    class Program
    {
        static void Main(string[] args)
        {
            var tollCalc = new TollCalculator();

            var car = new Car();
            var taxi = new Taxi();
            var bus = new Bus();
            var truck = new DeliveryTruck();

            Console.WriteLine($"The toll for a car is {tollCalc.CalculateToll(car)}");
            Console.WriteLine($"The toll for a taxi is {tollCalc.CalculateToll(taxi)}");
            Console.WriteLine($"The toll for a bus is {tollCalc.CalculateToll(bus)}");
            Console.WriteLine($"The toll for a truck is {tollCalc.CalculateToll(truck)}");

            try
            {
                tollCalc.CalculateToll("this will fail");
            }
            catch (ArgumentException e)
            {
                Console.WriteLine("Caught an argument exception when using the wrong type");
            }
            try
            {
                tollCalc.CalculateToll(null);
            }
            catch (ArgumentNullException e)
            {
                Console.WriteLine("Caught an argument exception when using null");
            }
        }
    }
}
```

<span data-ttu-id="2f700-165">Этот код включен в начальный проект, но закомментирован. Удалите символы комментария, чтобы проверить написанный код.</span><span class="sxs-lookup"><span data-stu-id="2f700-165">That code is included in the starter project, but is commented out. Remove the comments, and you can test what you've written.</span></span>

<span data-ttu-id="2f700-166">На этом примере можно понять, как шаблоны помогают создавать алгоритмы, в которых код и данные разделены.</span><span class="sxs-lookup"><span data-stu-id="2f700-166">You're starting to see how patterns can help you create algorithms where the code and the data are separate.</span></span> <span data-ttu-id="2f700-167">Выражение `switch` проверяет тип и создает различные значения в зависимости от результатов.</span><span class="sxs-lookup"><span data-stu-id="2f700-167">The `switch` expression tests the type and produces different values based on the results.</span></span> <span data-ttu-id="2f700-168">Но это только начало.</span><span class="sxs-lookup"><span data-stu-id="2f700-168">That's only the beginning.</span></span>

## <a name="add-occupancy-pricing"></a><span data-ttu-id="2f700-169">Добавление цен с учетом загруженности дороги</span><span class="sxs-lookup"><span data-stu-id="2f700-169">Add occupancy pricing</span></span>

<span data-ttu-id="2f700-170">Орган, взимающий сбор, поощряет передвижение автомобилей с максимальным количеством пассажиров.</span><span class="sxs-lookup"><span data-stu-id="2f700-170">The toll authority wants to encourage vehicles to travel at maximum capacity.</span></span> <span data-ttu-id="2f700-171">Следовательно, плата за проезд возрастает, когда в автомобиле находится меньше пассажиров, и наоборот:</span><span class="sxs-lookup"><span data-stu-id="2f700-171">They've decided to charge more when vehicles have fewer passengers, and encourage full vehicles by offering lower pricing:</span></span>

- <span data-ttu-id="2f700-172">Автомобили и такси без пассажиров должны заплатить дополнительные 0,50 долл. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-172">Cars and taxis with no passengers pay an extra $0.50.</span></span>
- <span data-ttu-id="2f700-173">Автомобили и такси с двумя пассажирами получают скидку 0,50 долл. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-173">Cars and taxis with two passengers get a 0.50 discount.</span></span>
- <span data-ttu-id="2f700-174">Автомобили и такси с тремя пассажирами получают скидку 1,00 долл. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-174">Cars and taxis with three or more passengers get a $1.00 discount.</span></span>
- <span data-ttu-id="2f700-175">Если автобус заполнен менее чем на половину, взимается дополнительная плата — 2,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-175">Buses that are less than 50% full pay an extra $2.00.</span></span>
- <span data-ttu-id="2f700-176">Если автобус заполнен более чем на 90 %, действует скидка 1,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-176">Buses that are more than 90% full get a $1.00 discount.</span></span>

<span data-ttu-id="2f700-177">Эти правила можно реализовать с помощью **шаблона свойств** в этом же выражении switch.</span><span class="sxs-lookup"><span data-stu-id="2f700-177">These rules can be implemented using the **property pattern** in the same switch expression.</span></span> <span data-ttu-id="2f700-178">Шаблон свойств проверяет свойства объекта после определения его типа.</span><span class="sxs-lookup"><span data-stu-id="2f700-178">The property pattern examines properties of the object once the type has been determined.</span></span> <span data-ttu-id="2f700-179">Один случай для `Car` включает четыре разных случая:</span><span class="sxs-lookup"><span data-stu-id="2f700-179">The single case for a `Car` expands to four different cases:</span></span>

```csharp
vehicle switch
{
    Car { Passengers: 0}        => 2.00m + 0.50m,
    Car { Passengers: 1 }       => 2.0m,
    Car { Passengers: 2}        => 2.0m - 0.50m,
    Car c                       => 2.00m - 1.0m,

    // ...
};
```

<span data-ttu-id="2f700-180">Первые три случая проверяют тип как `Car`, а затем проверяют значение свойства `Passengers`.</span><span class="sxs-lookup"><span data-stu-id="2f700-180">The first three cases test the type as a `Car`, then check the value of the `Passengers` property.</span></span> <span data-ttu-id="2f700-181">Если оба типа совпадают, это выражение вычисляется и возвращается результат.</span><span class="sxs-lookup"><span data-stu-id="2f700-181">If both match, that expression is evaluated and returned.</span></span>

<span data-ttu-id="2f700-182">Необходимо также реализовать аналогичные варианты для такси:</span><span class="sxs-lookup"><span data-stu-id="2f700-182">You would also expand the cases for taxis in a similar manner:</span></span>

```csharp
vehicle switch
{
    // ...

    Taxi { Fares: 0}  => 3.50m + 1.00m,
    Taxi { Fares: 1 } => 3.50m,
    Taxi { Fares: 2}  => 3.50m - 0.50m,
    Taxi t            => 3.50m - 1.00m,

    // ...
};
```

<span data-ttu-id="2f700-183">В предыдущем примере предложение `when` было опущено в последнем случае.</span><span class="sxs-lookup"><span data-stu-id="2f700-183">In the preceding example, the `when` clause was omitted on the final case.</span></span>

<span data-ttu-id="2f700-184">Затем реализуйте правила заполнения транспорта, расширив регистры для автобусов, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="2f700-184">Next, implement the occupancy rules by expanding the cases for buses, as shown in the following example:</span></span>

```csharp
vehicle switch
{
    // ...

    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m,
    Bus b => 5.00m,

    // ...
};
```

<span data-ttu-id="2f700-185">Орган, взимающий сборы, не учитывает количество пассажиров в грузовых автомобилях, выполняющих доставку.</span><span class="sxs-lookup"><span data-stu-id="2f700-185">The toll authority isn't concerned with the number of passengers in the delivery trucks.</span></span> <span data-ttu-id="2f700-186">Вместо этого, плата взимается с учетом веса грузовых автомобилей.</span><span class="sxs-lookup"><span data-stu-id="2f700-186">Instead, they charge more based on the weight class of the trucks.</span></span> <span data-ttu-id="2f700-187">Для грузовых автомобилей весом более 2268 кг взимается дополнительная плата в размере 5,00 долл. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-187">Trucks over 5000 lbs are charged an extra $5.00.</span></span> <span data-ttu-id="2f700-188">Для легких грузовиков весом до 1361 кг применяется скидка 2,00 долл. США.</span><span class="sxs-lookup"><span data-stu-id="2f700-188">Light trucks under 3000 lbs are given a $2.00 discount.</span></span> <span data-ttu-id="2f700-189">Это правило, реализуется с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="2f700-189">That rule is implemented with the following code:</span></span>

```csharp
vehicle switch
{
    // ...

    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,
};
```

<span data-ttu-id="2f700-190">В этом коде показано предложение `when` ветви switch.</span><span class="sxs-lookup"><span data-stu-id="2f700-190">The preceding code shows the `when` clause of a switch arm.</span></span> <span data-ttu-id="2f700-191">Используйте предложение `when` для проверки условий для свойства, отличающихся от равенства.</span><span class="sxs-lookup"><span data-stu-id="2f700-191">You use the `when` clause to test conditions other than equality on a property.</span></span> <span data-ttu-id="2f700-192">Завершив работу, вы получите метод, который выглядит приблизительно так:</span><span class="sxs-lookup"><span data-stu-id="2f700-192">When you've finished, you'll have a method that looks much like the following:</span></span>

```csharp
vehicle switch
{
    Car { Passengers: 0}        => 2.00m + 0.50m,
    Car { Passengers: 1}        => 2.0m,
    Car { Passengers: 2}        => 2.0m - 0.50m,
    Car c                       => 2.00m - 1.0m,

    Taxi { Fares: 0}  => 3.50m + 1.00m,
    Taxi { Fares: 1 } => 3.50m,
    Taxi { Fares: 2}  => 3.50m - 0.50m,
    Taxi t            => 3.50m - 1.00m,

    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m,
    Bus b => 5.00m,

    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,
};
```

<span data-ttu-id="2f700-193">Многие из этих вариантов являются примерами **рекурсивных шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="2f700-193">Many of these switch arms are examples of **recursive patterns**.</span></span> <span data-ttu-id="2f700-194">Например, в `Car { Passengers: 1}` показан шаблон константы внутри шаблона свойств.</span><span class="sxs-lookup"><span data-stu-id="2f700-194">For example, `Car { Passengers: 1}` shows a constant pattern inside a property pattern.</span></span>

<span data-ttu-id="2f700-195">Вы можете уменьшить количество повторяемых участков кода, использовав вложенные операторы switch.</span><span class="sxs-lookup"><span data-stu-id="2f700-195">You can make this code less repetitive by using nested switches.</span></span> <span data-ttu-id="2f700-196">В предыдущих примерах объекты `Car` и `Taxi` имеют по четыре варианта.</span><span class="sxs-lookup"><span data-stu-id="2f700-196">The `Car` and `Taxi` both have four different arms in the preceding examples.</span></span> <span data-ttu-id="2f700-197">В обоих случаях вы можете создать шаблон типа, который передается в шаблон свойства.</span><span class="sxs-lookup"><span data-stu-id="2f700-197">In both cases, you can create a type pattern that feeds into a property pattern.</span></span> <span data-ttu-id="2f700-198">Пример использования этого способа показан в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="2f700-198">This technique is shown in the following code:</span></span>

```csharp
public decimal CalculateToll(object vehicle) =>
    vehicle switch
    {
        Car c => c.Passengers switch
        {
            0 => 2.00m + 0.5m,
            1 => 2.0m,
            2 => 2.0m - 0.5m,
            _ => 2.00m - 1.0m
        },

        Taxi t => t.Fares switch
        {
            0 => 3.50m + 1.00m,
            1 => 3.50m,
            2 => 3.50m - 0.50m,
            _ => 3.50m - 1.00m
        },

        Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
        Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m,
        Bus b => 5.00m,

        DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
        DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
        DeliveryTruck t => 10.00m,

        { }  => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
        null => throw new ArgumentNullException(nameof(vehicle))
    };
```

<span data-ttu-id="2f700-199">В предыдущем примере применение рекурсивного выражения позволяет не использовать повторно ветви `Car` и `Taxi` с дочерними ветвями, которые проверяют значение свойства.</span><span class="sxs-lookup"><span data-stu-id="2f700-199">In the preceding sample, using a recursive expression means you don't repeat the `Car` and `Taxi` arms containing child arms that test the property value.</span></span> <span data-ttu-id="2f700-200">Этот метод не используется для ветвей `Bus` и `DeliveryTruck`, так как они представляют собой проверочные диапазоны для свойства, а не дискретные значения.</span><span class="sxs-lookup"><span data-stu-id="2f700-200">This technique isn't used for the `Bus` and `DeliveryTruck` arms because those arms are testing ranges for the property, not discrete values.</span></span>

## <a name="add-peak-pricing"></a><span data-ttu-id="2f700-201">Добавление цен с учетом пиковой нагрузки</span><span class="sxs-lookup"><span data-stu-id="2f700-201">Add peak pricing</span></span>

<span data-ttu-id="2f700-202">Для последней функции орган, взимающий сборы, хочет добавить цену с учетом пиковой нагрузки.</span><span class="sxs-lookup"><span data-stu-id="2f700-202">For the final feature, the toll authority wants to add time sensitive peak pricing.</span></span> <span data-ttu-id="2f700-203">Утром и вечером, когда дороги наиболее загружены, сумма сбора удваивается.</span><span class="sxs-lookup"><span data-stu-id="2f700-203">During the morning and evening rush hours, the tolls are doubled.</span></span> <span data-ttu-id="2f700-204">Это правило влияет только на движение в одном направлении: при въезде в город утром и на выезде вечером в час пик.</span><span class="sxs-lookup"><span data-stu-id="2f700-204">That rule only affects traffic in one direction: inbound to the city in the morning, and outbound in the evening rush hour.</span></span> <span data-ttu-id="2f700-205">В другое время в течение рабочего дня плата увеличивается на 50 %.</span><span class="sxs-lookup"><span data-stu-id="2f700-205">During other times during the workday, tolls increase by 50%.</span></span> <span data-ttu-id="2f700-206">Поздно ночью и ранним утром плата уменьшается на 25 %.</span><span class="sxs-lookup"><span data-stu-id="2f700-206">Late night and early morning, tolls are reduced by 25%.</span></span> <span data-ttu-id="2f700-207">В выходные дни взимается стандартная плата, независимо от времени суток.</span><span class="sxs-lookup"><span data-stu-id="2f700-207">During the weekend, it's the normal rate, regardless of the time.</span></span>

<span data-ttu-id="2f700-208">Для этой функции вы будете использовать сопоставление шаблонов, но вместе с другими методами.</span><span class="sxs-lookup"><span data-stu-id="2f700-208">You'll use pattern matching for this feature, but you'll integrate it with other techniques.</span></span> <span data-ttu-id="2f700-209">Вы можете создать одно выражение для сопоставления шаблонов, учитывающее все комбинации направления, дня недели и времени.</span><span class="sxs-lookup"><span data-stu-id="2f700-209">You could build a single pattern match expression that would account for all the combinations of direction, day of the week, and time.</span></span> <span data-ttu-id="2f700-210">Результат будет представлен в виде сложного выражения.</span><span class="sxs-lookup"><span data-stu-id="2f700-210">The result would be a complicated expression.</span></span> <span data-ttu-id="2f700-211">Такое выражение затрудняет чтение и понимание кода.</span><span class="sxs-lookup"><span data-stu-id="2f700-211">It would be hard to read and difficult to understand.</span></span> <span data-ttu-id="2f700-212">В таком случает будет сложнее обеспечить правильность результатов.</span><span class="sxs-lookup"><span data-stu-id="2f700-212">That makes it hard to ensure correctness.</span></span> <span data-ttu-id="2f700-213">Но вы можете объединить эти методы для создания набора значений, который кратко описывает все эти состояния.</span><span class="sxs-lookup"><span data-stu-id="2f700-213">Instead, combine those methods to build a tuple of values that concisely describes all those states.</span></span> <span data-ttu-id="2f700-214">Затем, используйте сопоставление шаблонов, чтобы вычислить множитель для платы за проезд.</span><span class="sxs-lookup"><span data-stu-id="2f700-214">Then use pattern matching to calculate a multiplier for the toll.</span></span> <span data-ttu-id="2f700-215">Кортеж содержит три дискретные условия:</span><span class="sxs-lookup"><span data-stu-id="2f700-215">The tuple contains three discrete conditions:</span></span>

- <span data-ttu-id="2f700-216">День — выходной или рабочий.</span><span class="sxs-lookup"><span data-stu-id="2f700-216">The day is either a weekday or a weekend.</span></span>
- <span data-ttu-id="2f700-217">Диапазон времени, за который взимается плата.</span><span class="sxs-lookup"><span data-stu-id="2f700-217">The band of time when the toll is collected.</span></span>
- <span data-ttu-id="2f700-218">Направление в город или за город.</span><span class="sxs-lookup"><span data-stu-id="2f700-218">The direction is into the city or out of the city</span></span>

<span data-ttu-id="2f700-219">В таблице ниже показаны комбинации входных значений и множителя для цены в часы пик:</span><span class="sxs-lookup"><span data-stu-id="2f700-219">The following table shows the combinations of input values and the peak pricing multiplier:</span></span>

| <span data-ttu-id="2f700-220">Day</span><span class="sxs-lookup"><span data-stu-id="2f700-220">Day</span></span>        | <span data-ttu-id="2f700-221">Время</span><span class="sxs-lookup"><span data-stu-id="2f700-221">Time</span></span>         | <span data-ttu-id="2f700-222">Направление</span><span class="sxs-lookup"><span data-stu-id="2f700-222">Direction</span></span> | <span data-ttu-id="2f700-223">Премиум</span><span class="sxs-lookup"><span data-stu-id="2f700-223">Premium</span></span> |
| ---------- | ------------ | --------- |--------:|
| <span data-ttu-id="2f700-224">День недели</span><span class="sxs-lookup"><span data-stu-id="2f700-224">Weekday</span></span>    | <span data-ttu-id="2f700-225">Утренний час пик</span><span class="sxs-lookup"><span data-stu-id="2f700-225">morning rush</span></span> | <span data-ttu-id="2f700-226">Въезд</span><span class="sxs-lookup"><span data-stu-id="2f700-226">inbound</span></span>   | <span data-ttu-id="2f700-227">x 2,00</span><span class="sxs-lookup"><span data-stu-id="2f700-227">x 2.00</span></span>  |
| <span data-ttu-id="2f700-228">День недели</span><span class="sxs-lookup"><span data-stu-id="2f700-228">Weekday</span></span>    | <span data-ttu-id="2f700-229">Утренний час пик</span><span class="sxs-lookup"><span data-stu-id="2f700-229">morning rush</span></span> | <span data-ttu-id="2f700-230">Выезд</span><span class="sxs-lookup"><span data-stu-id="2f700-230">outbound</span></span>  | <span data-ttu-id="2f700-231">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-231">x 1.00</span></span>  |
| <span data-ttu-id="2f700-232">День недели</span><span class="sxs-lookup"><span data-stu-id="2f700-232">Weekday</span></span>    | <span data-ttu-id="2f700-233">Дневное время</span><span class="sxs-lookup"><span data-stu-id="2f700-233">daytime</span></span>      | <span data-ttu-id="2f700-234">Въезд</span><span class="sxs-lookup"><span data-stu-id="2f700-234">inbound</span></span>   | <span data-ttu-id="2f700-235">x 1,50</span><span class="sxs-lookup"><span data-stu-id="2f700-235">x 1.50</span></span>  |
| <span data-ttu-id="2f700-236">День недели</span><span class="sxs-lookup"><span data-stu-id="2f700-236">Weekday</span></span>    | <span data-ttu-id="2f700-237">Дневное время</span><span class="sxs-lookup"><span data-stu-id="2f700-237">daytime</span></span>      | <span data-ttu-id="2f700-238">Выезд</span><span class="sxs-lookup"><span data-stu-id="2f700-238">outbound</span></span>  | <span data-ttu-id="2f700-239">x 1,50</span><span class="sxs-lookup"><span data-stu-id="2f700-239">x 1.50</span></span>  |
| <span data-ttu-id="2f700-240">День недели</span><span class="sxs-lookup"><span data-stu-id="2f700-240">Weekday</span></span>    | <span data-ttu-id="2f700-241">Вечерний час пик</span><span class="sxs-lookup"><span data-stu-id="2f700-241">evening rush</span></span> | <span data-ttu-id="2f700-242">Въезд</span><span class="sxs-lookup"><span data-stu-id="2f700-242">inbound</span></span>   | <span data-ttu-id="2f700-243">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-243">x 1.00</span></span>  |
| <span data-ttu-id="2f700-244">День недели</span><span class="sxs-lookup"><span data-stu-id="2f700-244">Weekday</span></span>    | <span data-ttu-id="2f700-245">Вечерний час пик</span><span class="sxs-lookup"><span data-stu-id="2f700-245">evening rush</span></span> | <span data-ttu-id="2f700-246">Выезд</span><span class="sxs-lookup"><span data-stu-id="2f700-246">outbound</span></span>  | <span data-ttu-id="2f700-247">x 2,00</span><span class="sxs-lookup"><span data-stu-id="2f700-247">x 2.00</span></span>  |
| <span data-ttu-id="2f700-248">День недели</span><span class="sxs-lookup"><span data-stu-id="2f700-248">Weekday</span></span>    | <span data-ttu-id="2f700-249">Ночное время</span><span class="sxs-lookup"><span data-stu-id="2f700-249">overnight</span></span>    | <span data-ttu-id="2f700-250">Въезд</span><span class="sxs-lookup"><span data-stu-id="2f700-250">inbound</span></span>   | <span data-ttu-id="2f700-251">x 0,75</span><span class="sxs-lookup"><span data-stu-id="2f700-251">x 0.75</span></span>  |
| <span data-ttu-id="2f700-252">День недели</span><span class="sxs-lookup"><span data-stu-id="2f700-252">Weekday</span></span>    | <span data-ttu-id="2f700-253">Ночное время</span><span class="sxs-lookup"><span data-stu-id="2f700-253">overnight</span></span>    | <span data-ttu-id="2f700-254">Выезд</span><span class="sxs-lookup"><span data-stu-id="2f700-254">outbound</span></span>  | <span data-ttu-id="2f700-255">x 0,75</span><span class="sxs-lookup"><span data-stu-id="2f700-255">x 0.75</span></span>  |
| <span data-ttu-id="2f700-256">Выходные</span><span class="sxs-lookup"><span data-stu-id="2f700-256">Weekend</span></span>    | <span data-ttu-id="2f700-257">Утренний час пик</span><span class="sxs-lookup"><span data-stu-id="2f700-257">morning rush</span></span> | <span data-ttu-id="2f700-258">Въезд</span><span class="sxs-lookup"><span data-stu-id="2f700-258">inbound</span></span>   | <span data-ttu-id="2f700-259">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-259">x 1.00</span></span>  |
| <span data-ttu-id="2f700-260">Выходные</span><span class="sxs-lookup"><span data-stu-id="2f700-260">Weekend</span></span>    | <span data-ttu-id="2f700-261">Утренний час пик</span><span class="sxs-lookup"><span data-stu-id="2f700-261">morning rush</span></span> | <span data-ttu-id="2f700-262">Выезд</span><span class="sxs-lookup"><span data-stu-id="2f700-262">outbound</span></span>  | <span data-ttu-id="2f700-263">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-263">x 1.00</span></span>  |
| <span data-ttu-id="2f700-264">Выходные</span><span class="sxs-lookup"><span data-stu-id="2f700-264">Weekend</span></span>    | <span data-ttu-id="2f700-265">Дневное время</span><span class="sxs-lookup"><span data-stu-id="2f700-265">daytime</span></span>      | <span data-ttu-id="2f700-266">Въезд</span><span class="sxs-lookup"><span data-stu-id="2f700-266">inbound</span></span>   | <span data-ttu-id="2f700-267">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-267">x 1.00</span></span>  |
| <span data-ttu-id="2f700-268">Выходные</span><span class="sxs-lookup"><span data-stu-id="2f700-268">Weekend</span></span>    | <span data-ttu-id="2f700-269">Дневное время</span><span class="sxs-lookup"><span data-stu-id="2f700-269">daytime</span></span>      | <span data-ttu-id="2f700-270">Выезд</span><span class="sxs-lookup"><span data-stu-id="2f700-270">outbound</span></span>  | <span data-ttu-id="2f700-271">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-271">x 1.00</span></span>  |
| <span data-ttu-id="2f700-272">Выходные</span><span class="sxs-lookup"><span data-stu-id="2f700-272">Weekend</span></span>    | <span data-ttu-id="2f700-273">Вечерний час пик</span><span class="sxs-lookup"><span data-stu-id="2f700-273">evening rush</span></span> | <span data-ttu-id="2f700-274">Въезд</span><span class="sxs-lookup"><span data-stu-id="2f700-274">inbound</span></span>   | <span data-ttu-id="2f700-275">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-275">x 1.00</span></span>  |
| <span data-ttu-id="2f700-276">Выходные</span><span class="sxs-lookup"><span data-stu-id="2f700-276">Weekend</span></span>    | <span data-ttu-id="2f700-277">Вечерний час пик</span><span class="sxs-lookup"><span data-stu-id="2f700-277">evening rush</span></span> | <span data-ttu-id="2f700-278">Выезд</span><span class="sxs-lookup"><span data-stu-id="2f700-278">outbound</span></span>  | <span data-ttu-id="2f700-279">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-279">x 1.00</span></span>  |
| <span data-ttu-id="2f700-280">Выходные</span><span class="sxs-lookup"><span data-stu-id="2f700-280">Weekend</span></span>    | <span data-ttu-id="2f700-281">Ночное время</span><span class="sxs-lookup"><span data-stu-id="2f700-281">overnight</span></span>    | <span data-ttu-id="2f700-282">Въезд</span><span class="sxs-lookup"><span data-stu-id="2f700-282">inbound</span></span>   | <span data-ttu-id="2f700-283">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-283">x 1.00</span></span>  |
| <span data-ttu-id="2f700-284">Выходные</span><span class="sxs-lookup"><span data-stu-id="2f700-284">Weekend</span></span>    | <span data-ttu-id="2f700-285">Ночное время</span><span class="sxs-lookup"><span data-stu-id="2f700-285">overnight</span></span>    | <span data-ttu-id="2f700-286">Выезд</span><span class="sxs-lookup"><span data-stu-id="2f700-286">outbound</span></span>  | <span data-ttu-id="2f700-287">x 1,00</span><span class="sxs-lookup"><span data-stu-id="2f700-287">x 1.00</span></span>  |

<span data-ttu-id="2f700-288">Для этих трех переменных существует 16 разных комбинаций.</span><span class="sxs-lookup"><span data-stu-id="2f700-288">There are 16 different combinations of the three variables.</span></span> <span data-ttu-id="2f700-289">Комбинируя некоторые условия, вы упростите окончательное выражение switch.</span><span class="sxs-lookup"><span data-stu-id="2f700-289">By combining some of the conditions, you'll simplify the final switch expression.</span></span>

<span data-ttu-id="2f700-290">В системе для сбора платы структура <xref:System.DateTime> используется для определения времени сбора.</span><span class="sxs-lookup"><span data-stu-id="2f700-290">The system that collects the tolls uses a <xref:System.DateTime> structure for the time when the toll was collected.</span></span> <span data-ttu-id="2f700-291">Выполните методы-члены, которые создают переменные из предыдущей таблицы.</span><span class="sxs-lookup"><span data-stu-id="2f700-291">Build member methods that create the variables from the preceding table.</span></span> <span data-ttu-id="2f700-292">В следующей функции для сопоставления шаблонов используется выражение switch, позволяющее определить, соответствует ли <xref:System.DateTime> выходным или рабочим дням недели:</span><span class="sxs-lookup"><span data-stu-id="2f700-292">The following function uses a pattern matching switch expression to express whether a <xref:System.DateTime> represents a weekend or a weekday:</span></span>

```csharp
private static bool IsWeekDay(DateTime timeOfToll) =>
    timeOfToll.DayOfWeek switch
    {
        DayOfWeek.Monday    => true,
        DayOfWeek.Tuesday   => true,
        DayOfWeek.Wednesday => true,
        DayOfWeek.Thursday  => true,
        DayOfWeek.Friday    => true,
        DayOfWeek.Saturday  => false,
        DayOfWeek.Sunday    => false
    };
```

<span data-ttu-id="2f700-293">Этот метод работает, но фрагменты кода повторяются.</span><span class="sxs-lookup"><span data-stu-id="2f700-293">That method works, but it's repetitious.</span></span> <span data-ttu-id="2f700-294">Вы можете упростить его, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="2f700-294">You can simplify it, as shown in the following code:</span></span>

[!code-csharp[IsWeekDay](~/samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#IsWeekDay)]

<span data-ttu-id="2f700-295">Затем добавьте такую же функцию, чтобы создать временные интервалы:</span><span class="sxs-lookup"><span data-stu-id="2f700-295">Next, add a similar function to categorize the time into the blocks:</span></span>

[!code-csharp[GetTimeBand](~/samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#GetTimeBand)]

<span data-ttu-id="2f700-296">В предыдущем методе сопоставление шаблонов не применяется.</span><span class="sxs-lookup"><span data-stu-id="2f700-296">The previous method doesn't use pattern matching.</span></span> <span data-ttu-id="2f700-297">В этом случае проще использовать каскад операторов `if`.</span><span class="sxs-lookup"><span data-stu-id="2f700-297">It's clearer using a familiar cascade of `if` statements.</span></span> <span data-ttu-id="2f700-298">Чтобы преобразовать каждый диапазон времени в дискретную величину, добавьте закрытое перечисление `enum`.</span><span class="sxs-lookup"><span data-stu-id="2f700-298">You do add a private `enum` to convert each range of time to a discrete value.</span></span>

<span data-ttu-id="2f700-299">Создав эти методы, можно использовать другое выражение `switch` с **шаблоном кортежа** для вычисления премиум-цены.</span><span class="sxs-lookup"><span data-stu-id="2f700-299">After you create those methods, you can use another `switch` expression with the **tuple pattern** to calculate the pricing premium.</span></span> <span data-ttu-id="2f700-300">Вы можете записать выражение `switch` сразу со всеми 16 вариантами:</span><span class="sxs-lookup"><span data-stu-id="2f700-300">You could build a `switch` expression with all 16 arms:</span></span>

[!code-csharp[FullTuplePattern](~/samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#TuplePatternOne)]

<span data-ttu-id="2f700-301">Код выше работает, но его можно упростить.</span><span class="sxs-lookup"><span data-stu-id="2f700-301">The above code works, but it can be simplified.</span></span> <span data-ttu-id="2f700-302">Плата для всех восьми сочетаний в выходные дни одинаковая.</span><span class="sxs-lookup"><span data-stu-id="2f700-302">All eight combinations for the weekend have the same toll.</span></span> <span data-ttu-id="2f700-303">Вы можете заменить все восемь вариантов одной строкой:</span><span class="sxs-lookup"><span data-stu-id="2f700-303">You can replace all eight with the following line:</span></span>

```csharp
(false, _, _) => 1.0m,
```

<span data-ttu-id="2f700-304">Для входящего и исходящего трафика используется одинаковый множитель в дневное и ночное время в рабочие дни.</span><span class="sxs-lookup"><span data-stu-id="2f700-304">Both inbound and outbound traffic have the same multiplier during the weekday daytime and overnight hours.</span></span> <span data-ttu-id="2f700-305">Эти четыре случая можно заменить следующими двумя строками:</span><span class="sxs-lookup"><span data-stu-id="2f700-305">Those four switch arms can be replaced with the following two lines:</span></span>

```csharp
(true, TimeBand.Overnight, _) => 0.75m,
(true, TimeBand.Daytime, _)   => 1.5m,
```

<span data-ttu-id="2f700-306">После этих двух изменений код должен выглядеть как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="2f700-306">The code should look like the following code after those two changes:</span></span>

```csharp
public decimal PeakTimePremium(DateTime timeOfToll, bool inbound) =>
    (IsWeekDay(timeOfToll), GetTimeBand(timeOfToll), inbound) switch
    {
        (true, TimeBand.MorningRush, true)  => 2.00m,
        (true, TimeBand.MorningRush, false) => 1.00m,
        (true, TimeBand.Daytime,     _)     => 1.50m,
        (true, TimeBand.EveningRush, true)  => 1.00m,
        (true, TimeBand.EveningRush, false) => 2.00m,
        (true, TimeBand.Overnight,   _)     => 0.75m,
        (false, _,                   _)     => 1.00m,
    };
```

<span data-ttu-id="2f700-307">Наконец, вы можете удалить два часа пик, за которые взимается обычная плата.</span><span class="sxs-lookup"><span data-stu-id="2f700-307">Finally, you can remove the two rush hour times that pay the regular price.</span></span> <span data-ttu-id="2f700-308">Удалив эти ветви, можно заменить `false` пустой переменной (`_`) в последней ветви switch.</span><span class="sxs-lookup"><span data-stu-id="2f700-308">Once you remove those arms, you can replace the `false` with a discard (`_`) in the final switch arm.</span></span> <span data-ttu-id="2f700-309">У вас получится следующий законченный метод:</span><span class="sxs-lookup"><span data-stu-id="2f700-309">You'll have the following finished method:</span></span>

[!code-csharp[SimplifiedTuplePattern](../../../samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#FinalTuplePattern)]

<span data-ttu-id="2f700-310">В этом примере продемонстрировано одно из преимуществ сопоставления шаблонов: ветви шаблона вычисляются по порядку.</span><span class="sxs-lookup"><span data-stu-id="2f700-310">This example highlights one of the advantages of pattern matching: the pattern branches are evaluated in order.</span></span> <span data-ttu-id="2f700-311">Если поменять их местами так, чтобы начальная ветвь обрабатывала один из последних случаев, компилятор выдаст предупреждение о недостижимом коде.</span><span class="sxs-lookup"><span data-stu-id="2f700-311">If you rearrange them so that an earlier branch handles one of your later cases, the compiler warns you about the unreachable code.</span></span> <span data-ttu-id="2f700-312">Эти правила языка облегчают реализацию упрощений, выполненных выше, и гарантируют, что код не изменится.</span><span class="sxs-lookup"><span data-stu-id="2f700-312">Those language rules made it easier to do the preceding simplifications with confidence that the code didn't change.</span></span>

<span data-ttu-id="2f700-313">Сопоставление шаблонов делает код некоторых типов более удобочитаемым и предлагает альтернативу объектно-ориентированным методам, когда добавление кода в классы невозможно.</span><span class="sxs-lookup"><span data-stu-id="2f700-313">Pattern matching makes some types of code more readable and offers an alternative to object-oriented techniques when you can't add code to your classes.</span></span> <span data-ttu-id="2f700-314">В облаке данные и функции хранятся рядом.</span><span class="sxs-lookup"><span data-stu-id="2f700-314">The cloud is causing data and functionality to live apart.</span></span> <span data-ttu-id="2f700-315">*Форма* данных и *операции* в облаке не обязательно описываются вместе.</span><span class="sxs-lookup"><span data-stu-id="2f700-315">The *shape* of the data and the *operations* on it aren't necessarily described together.</span></span> <span data-ttu-id="2f700-316">В этом руководстве показано, что существующие данные можно использовать совершенно иначе, чем в первоначальной функции.</span><span class="sxs-lookup"><span data-stu-id="2f700-316">In this tutorial, you consumed existing data in entirely different ways from its original function.</span></span> <span data-ttu-id="2f700-317">Сопоставление шаблонов позволяет создавать функции с переопределением типов, хотя их не удается расширить.</span><span class="sxs-lookup"><span data-stu-id="2f700-317">Pattern matching gave you the ability to write functionality that overrode those types, even though you couldn't extend them.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f700-318">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="2f700-318">Next steps</span></span>

<span data-ttu-id="2f700-319">Скачать готовый код можно из репозитория GitHub [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/finished).</span><span class="sxs-lookup"><span data-stu-id="2f700-319">You can download the finished code from the [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/finished) GitHub repository.</span></span> <span data-ttu-id="2f700-320">Изучите шаблоны самостоятельно и используйте эту методику во время написания кода.</span><span class="sxs-lookup"><span data-stu-id="2f700-320">Explore patterns on your own and add this technique into your regular coding activities.</span></span> <span data-ttu-id="2f700-321">Это позволит вам подойти к решению проблем с другой стороны, чтобы создавать новые функции.</span><span class="sxs-lookup"><span data-stu-id="2f700-321">Learning these techniques gives you another way to approach problems and create new functionality.</span></span>