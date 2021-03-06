---
title: Реляционные и Данные NoSQL
description: Узнайте о реляционных данных и данных NoS'L в облачных приложениях
author: robvet
ms.date: 01/22/2020
ms.openlocfilehash: 3fb3dcc3a87e278c05f3e15d261245f4d61453d1
ms.sourcegitcommit: f87ad41b8e62622da126aa928f7640108c4eff98
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "80805806"
---
# <a name="relational-vs-nosql-data"></a>Реляционные и Данные NoSQL

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

Relational и NoS'L — это два типа систем баз данных, обычно внедряемых в облачных приложениях. Они построены по-разному, хранят данные по-разному, и доступ по-разному. В этом разделе мы рассмотрим и то, и другое. Позже в этой главе мы рассмотрим новую технологию баз данных под названием *NewS'SL*.

*Реляционные базы данных* были распространенной технологией на протяжении десятилетий. Они зрелые, проверенные и широко реализованные. Конкурирующие продукты баз данных, инструментарий и опыт изобилуют. Реляционные базы данных обеспечивают хранилище соответствующих таблиц данных. Эти таблицы имеют фиксированную схему, используйте S'L (структурированный язык запросов) для управления данными и поддерживают гарантии ACID.

*Базы данных без СЗЛ* относятся к высокопроизводительным, нерелоняционным хранилищам данных. Они преуспевают в своих характеристиках простоты использования, масштабируемости, устойчивости и доступности. Вместо присоединения к таблицам нормализованных данных, НоСЗЛ хранит неструктурированные или полуструктурированные данные, часто в парах с ключевыми значениями или документами JSON. Базы данных без СЗЛ, как правило, не предоставляют acid гарантии, выходящие за рамки единого раздела базы данных. Услуги высокого объема, требующие второго времени отклика, отдают предпочтение хранилищам данных NoS'L.

Влияние технологий [НоСЗЛ](https://www.geeksforgeeks.org/introduction-to-nosql/) на распределенные облачные системы невозможно переоценить. Распространение новых технологий передачи данных в этом пространстве привело к нарушению решений, которые когда-то опирались исключительно на реляционные базы данных.

Базы данных NoS'L включают в себя несколько различных моделей для доступа к данным и управления ими, каждая из которых подходит для конкретных случаев использования. На рисунке 5-9 представлены четыре общие модели.

![Модели данных NoS'L](./media/types-of-nosql-datastores.png)

**Рисунок 5-9**: Модели данных для баз данных НоСЗЛ

| Модель | Характеристики |
| :-------- | :-------- |
| Хранилище документов | Данные и метаданные хранятся иерархически в документах на основе JSON внутри базы данных. |
| Магазин добавленных значений | Самые простые из баз данных НоСЗЛ, данные представлены как набор пар с ключевыми значениями. |
| Широкий колонок магазин | Связанные данные хранятся в виде набора вложенных ключей/значенийных пар в одном столбце. |
| Графический магазин | Данные хранятся в структуре графика в виде свойств узлов, краев и данных. |

## <a name="the-cap-theorem"></a>Теорема CAP

Чтобы понять различия между этими типами баз данных, рассмотрим теорему CAP, набор принципов, применяемых к распределенным системам, которые хранят состояние. На рисунке 5-10 показаны три свойства теоремы CAP.

![Теорема CAP](./media/cap-theorem.png)

**Рис. 5-10**. Теорема CAP

В теореме говорится, что распределенные системы данных будут предлагать компромисс между согласованностью, доступностью и допуском разделов. И, что любая база данных может гарантировать только *два* из трех свойств:

- *Согласованности.* Каждый узла в кластере отвечает самым последним данным, даже если система должна заблокировать запрос до тех пор, пока все реплики не будут обновлены. Если вы запросите «согласованную систему» для элемента, который в настоящее время обновляется, вы будете ждать этого ответа, пока все реплики успешно не обновятся. Тем не менее, вы получите самые актуальные данные.

- *Доступности.* Каждый узла возвращает немедленный ответ, даже если этот ответ не является самым последним и последними данными. Если вы запросите «доступную систему» для обновления товара, вы получите наилучший ответ, который служба может предоставить в этот момент.

- *Толерантность к разделу.* Гарантирует, что система продолжает работать, даже если реплицированный узла данных выходит из строя или теряет связь с другими реплицированными узлами данных.

Реляционные базы данных обычно обеспечивают согласованность и доступность, но не допуск раздела. Они обычно готовятся к одному серверу и масштабируются вертикально, добавляя больше ресурсов в машину.

Многие реляционные системы баз данных поддерживают встроенные функции репликации, в которых копии первичной базы данных могут быть сделаны в другие экземпляры вторичного сервера. Операции записи производятся в основной экземпляр и реплицируются каждому из второстепенных. При сбое основной экземпляр может выйти на вторичку, чтобы обеспечить высокую доступность. Вторые также могут быть использованы для распространения считываемых операций. В то время как операции записей всегда идут вразрез с основной репликой, считываемые операции могут быть направлены на любой из второстепенных для снижения загрузки системы.

Данные также могут быть горизонтально разделены на несколько узлов, например, с [осколками.](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-introduction) Но, sharding резко увеличивает операционные накладные расходы, плюя данные по многим частям, которые не могут легко общаться. Управление может быть дорогостоящим и отнимает много времени. Это может в конечном итоге повлиять на производительность, таблицы соединения, и референциальной целостности.

Если реплики данных потеряют подключение к сети в "высокосогласованном" реляционном кластере базы данных, вы не сможете написать в базу данных. Система откажется от операции записи, так как не может воспроизвести это изменение в другой реплике данных. Каждая реплика данных должна обновляться до завершения транзакции.

Базы данных NoS'L обычно поддерживают высокую доступность и допуск разделов. Они масштабируются горизонтально, часто через товарные серверы. Такой подход обеспечивает огромную доступность как внутри географических регионов, так и между регионами по сниженным ценам. Вы разделите и воспроизводите данные на этих машинах или узлах, обеспечивая избыточность и допуск неисправностей. Недостатком является последовательность. Изменение данных на одном узле НоСЗЛ может занять некоторое время для распространения на другие узлы. Как правило, узл базы данных NoS'L обеспечивает немедленный ответ на запрос, даже если представленные данные устарели и еще не обновлены.

Если реплики данных потеряют подключение в «высокодоступном» кластере базы данных НоСЗЛ, все равно можно завершить операцию записи в базу данных. Кластер базы данных позволит операции записи и обновлять каждую реплику данных по мере ее появления.

Этот результат известен как конечная согласованность, характеристика распределенных систем данных, где транзакции ACID не поддерживаются. Это краткая задержка между обновлением элемента данных и временем, которое требуется для распространения обновления каждого из узлов реплики. В нормальных условиях, отставание, как правило, короткий, но может увеличиваться, когда возникают проблемы. Например, что произойдет, если обновить элемент продукта в базе данных НоСЗЛ в США и запросить тот же элемент данных из узла реплики в Европе? Вы получите более раннюю информацию о продукте до тех пор, пока кластер не обновит европейский узлы с изменением продукта. Немедленно вернув результат запроса и не дожидаясь обновления всех узлов реплики, вы приобретаете огромный масштаб и объем, но с возможностью представления старых данных.

Высокая доступность и массовая масштабируемость часто имеют более важное значение для бизнеса, чем сильная согласованность. Разработчики могут внедрять методы и шаблоны, такие как Sagas, C'RS и асинхронные сообщения, чтобы охватить окончательную согласованность.

> В настоящее время, осторожность должна быть принята при потворствух CAP теоремы ограничений. Появился новый тип базы данных, который называется NewS'L, который расширяет реляционную базу данных для поддержки как горизонтальной масштабируемости, так и масштабируемой производительности систем НоСЗЛ.

## <a name="considerations-for-relational-vs-nosql-systems"></a>Рассмотрение реляционных и ноСЗЛ систем

Основываясь на конкретных требованиях к данным, микрослужба на основе облачных технологий может реализовывать реляционный, ноСЗЛ-хранилище данных или и то, и другое.

|  Рассмотрим хранилище данных NoS'L, когда: | Рассмотрим реляционную базу данных, когда: |
| :-------- | :-------- |
| У вас есть большие объемные нагрузки, которые требуют больших масштабов | Объем рабочей нагрузки соответствует и требует среднего и крупного масштаба |
| Ваши рабочие нагрузки не требуют гарантий ACID |  Требуются гарантии ACID |
| Ваши данные динамичны и часто меняются | Ваши данные предсказуемы и высоко структурированы |
| Данные могут быть выражены без связи | Данные лучше всего выражаются в реляционно |  
| Вам нужно быстро писать и писать безопасности не является критическим | Безопасность написания является обязательным требованием |  
| Поиск данных прост и имеет тенденцию быть плоским | Вы работаете со сложными запросами и отчетами|
| Ваши данные требуют широкого географического распространения | Ваши пользователи более централизованы |
| Ваше приложение будет развернуто на товарном оборудовании, например, в общедоступных облаках | Ваше приложение будет развернуто на большом и высококачественном оборудовании |
|||

В следующих разделах мы рассмотрим варианты, доступные в облаке Azure для хранения и управления облачными данными.

## <a name="database-as-a-service"></a>База данных как услуга

Для начала можно предоставить виртуальную машину Azure и установить подвыборную базу данных для каждой службы. Хотя вы будете иметь полный контроль над окружающей средой, вы бы откавылись от многих встроенных функций облачной платформы. Вы также будете отвечать за управление виртуальной машиной и базой данных для каждой службы. Такой подход может быстро стать трудоемким и дорогостоящим.

Вместо этого облачные приложения отдают предпочтение службам данных, которые рассматриваются как [база данных как служба (DBaaS).](https://www.stratoscale.com/blog/dbaas/what-is-database-as-a-service/) Эти службы, полностью управляемые облачным поставщиком, обеспечивают встроенную безопасность, масштабируемость и мониторинг. Вместо того, чтобы владеть услугой, вы просто потребляете ее в качестве [резервной службы.](./definition.md#backing-services) Поставщик управляет ресурсом в масштабе и несет ответственность за производительность и техническое обслуживание.

Они могут быть настроены в зонах доступности облаков и регионах для достижения высокой доступности. Все они поддерживают просто в срок емкости и платить по мере вас-го модели. Azure имеет различные виды управляемых вариантов обслуживания данных, каждый из которых имеет определенные преимущества.

Сначала рассмотрим реляционные dBaaS-службы, доступные в Azure. Вы увидите, что флагманская база данных Microsoft s'L Server доступна вместе с несколькими вариантами с открытым исходным кодом. Затем мы поговорим об службах данных NoS'L в Azure.

## <a name="azure-relational-databases"></a>Реляционные базы данных Azure

Для облачных микрослужб, требующих реляционных данных, Azure предлагает четыре управляемые реляционные базы данных в качестве услуги (DBaaS), показанные на рисунке 5-11.

![Управляемые реляционные базы данных в Azure](./media/azure-managed-databases.png)

**Рис. 5-11**. Управляемые реляционные базы данных, доступные в Azure

На предыдущем рисунке обратите внимание на то, как каждый из них находится на общей инфраструктуре DBaaS, которая имеет ключевые возможности без каких-либо дополнительных затрат.

Эти функции особенно важны для организаций, которые предоставляет большое количество баз данных, но имеют ограниченные ресурсы для их управления.
Базу данных Azure можно предоставить за считанные минуты, выбрав объем обрабатывающих ядер, память и базовое хранилище. Вы можете масштабировать базу данных на лету и динамически корректировать ресурсы практически без простоев.

## <a name="azure-sql-database"></a>База данных SQL Azure

Команды разработчиков, имеющих опыт работы в microsoft S'L Server, должны рассмотреть [базу данных Azure S'L.](https://docs.microsoft.com/azure/sql-database/) Это полностью управляемая реляционная база данных как услуга (DBaaS) на основе серверной базы данных Microsoft S'L. Служба разделяет множество функций, найденных в предварительной версии сервера S'L, и запускает последнюю стабильную версию серверного движка базы данных S'L.

Для использования в облачной микрослужбе база данных Azure S'L доступна с тремя вариантами развертывания:

- Единая база данных представляет собой полностью управляемую базу данных S'L, работающего на [сервере базы данных Azure s'L](https://docs.microsoft.com/azure/sql-database/sql-database-servers) в облаке Azure. База данных считается [*содержащейся,*](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) поскольку она не имеет зависимостей конфигурации от базового сервера базы данных.
  
- [Управляемая инстанция](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) — это полностью управляемый экземпляр серверной базы данных Microsoft S'L, обеспечивающий почти 100% совместимость с предприимчивым сервером S'L. Эта опция поддерживает более крупные базы данных, до 35 ТБ, и помещается в [виртуальную сеть Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) для лучшей изоляции.

- [База данных Azure s'L без сервера](https://docs.microsoft.com/azure/sql-database/sql-database-serverless) — это вычислительный уровень для одной базы данных, который автоматически масштабируется в зависимости от спроса на рабочую нагрузку. Он выставляет счета только за количество вычислений, используемых в секунду. Услуга хорошо подходит для рабочих нагрузок с прерывистыми, непредсказуемыми моделями использования, перемежаются с периодами бездействия. Уровень без сервера вычисляет также автоматически приостанавливает базы данных во время неактивных периодов, так что выставляются только платежи за хранение. Он автоматически возобновляется при возврате действия.

Помимо традиционного стека серверов Microsoft S'L Server, Azure также имеет управляемые версии трех популярных баз данных с открытым исходным кодом.

## <a name="open-source-databases-in-azure"></a>Базы данных с открытым исходным кодом в Azure

Реляционные базы данных с открытым исходным кодом стали популярным выбором для облачных приложений. Многие предприятия отдают им предпочтение по сравнению с коммерческими продуктами баз данных, особенно для экономии средств. Многие группы разработчиков пользуются гибкостью, развитием, поддерживаемым сообществом, а также экосистемой инструментов и расширений. Базы данных с открытым исходным кодом могут быть развернуты в нескольких облачных провайдерах, что помогает свести к минимуму проблему блокировки поставщиков.

Разработчики могут легко самостоятельно разместить любую базу данных с открытым исходным кодом на Azure VM. Обеспечивая полный контроль, этот подход ставит вас на крючок для управления, мониторинга и обслуживания базы данных и VM.

Тем не менее, корпорация Майкрософт продолжает свою приверженность сохранению Azure «открытой платформы», предлагая несколько популярных баз данных с открытым исходным кодом в качестве *полностью управляемых* служб DBaaS.

### <a name="azure-database-for-mysql"></a>База данных Azure для MySQL

[MyS'L](https://en.wikipedia.org/wiki/MySQL) — это реляционная база данных с открытым исходным кодом и столп для приложений, построенных на [стеку программного обеспечения LAMP.](https://en.wikipedia.org/wiki/LAMP_(software_bundle)) Широко подобраны для *чтения тяжелых* рабочих нагрузок, он используется многими крупными организациями, в том числе Facebook, Twitter и YouTube. Сообщество издание доступно бесплатно, в то время как предприятие издание требует покупки лицензии. Первоначально созданный в 1995 году, продукт был приобретен Sun Microsystems в 2008 году. Oracle приобрела Sun и MyS'L в 2010 году.

[База данных Azure для MyS'L](https://azure.microsoft.com/services/mysql/) — это управляемая реляционная служба баз данных, основанная на движке MyS'L Server с открытым исходным кодом. В нем используется издание сообщества MyS'L. Сервер Azure MyS'L является административной точкой службы. Это тот же серверный движок MyS'L, используемый для развертывания на месте. Двигатель может создавать единую базу данных на сервер или несколько баз данных на сервер, которые разделяют ресурсы. Вы можете продолжать управлять данными с помощью тех же инструментов с открытым исходным кодом, не осваивая новые навыки или управляя виртуальными машинами.

### <a name="azure-database-for-mariadb"></a>База данных Azure для MariaDB

[MariaDB](https://mariadb.com/) Сервер является еще одним популярным сервером баз данных с открытым исходным кодом. Он был создан в качестве *вилки* MyS'L, когда Oracle приобрела Sun Microsystems, который владел MyS'L. Цель состояла в том, чтобы гарантировать, что MariaDB остается открытым исходным кодом. Поскольку MariaDB является вилкой MyS'L, определения данных и таблицсовместимы, а клиентские протоколы, структуры и AI являются сплоченными.

MariaDB имеет сильное сообщество и используется многими крупными предприятиями. В то время как Oracle продолжает поддерживать, укреплять и поддерживать MyS'L, фонд MariaDB управляет MariaDB, позволяя публичный вклад в продукт и документацию.

[База данных Azure для MariaDB](https://azure.microsoft.com/services/mariadb/) — это полностью управляемая реляционная база данных как служба в облаке Azure. Услуга основана на серверном движке серверного выпуска сообщества MariaDB. Он может обрабатывать критически важные рабочие нагрузки с предсказуемой производительностью и динамической масштабируемостью.

### <a name="azure-database-for-postgresql"></a>База данных Azure для PostgreSQL

[PostgreS'L](https://www.postgresql.org/) является реляционной базой данных с открытым исходным кодом с более чем 30-летним активным развитием. PostgresSSL имеет прочную репутацию надежности и целостности данных. Это многофункциональный, совместимый с функцией, и считается более исполнительным, чем MyS'L - особенно для рабочих нагрузок со сложными запросами и тяжелыми пишет. Многие крупные предприятия, втоместь Apple, Red Hat и Fujitsu, создали продукты с использованием PostgreS'L.

[База данных Azure для PostgreS'L](https://azure.microsoft.com/services/postgresql/) — это полностью управляемая реляционная служба баз данных, основанная на движке базы данных Postgres с открытым исходным кодом. Сервис поддерживает многие платформы разработки, в том числе СЗ, Ява, Python, Узла, C\#и PHP. Вы можете перенести в него базы данных PostgreS'L с помощью инструмента [интерфейса командной строки](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql?step=1) или службы миграции данных Azure.

База данных Azure для PostgreS'L доступна с двумя вариантами развертывания:

- Опция развертывания [Единого сервера](https://docs.microsoft.com/azure/postgresql/concepts-servers) является центральной административной точкой для нескольких баз данных, в которые можно развернуть множество баз данных. Ценообразование структурировано на сервер на основе ядер и хранения.

- Гипермасштаб [(Citus) вариант](https://azure.microsoft.com/blog/get-high-performance-scaling-for-your-azure-database-workloads-with-hyperscale/) питается от технологии Citus Data. Это позволяет повысить производительность за счет *горизонтального масштабирования* единой базы данных на сотнях узлов для обеспечения быстрой производительности и масштабирования. Эта опция позволяет движку вписывать больше данных в память, параллелизировать запросы в сотнях узлов и быстрее индексировать данные.

## <a name="nosql-data-in-azure"></a>Данные NoS'L в Azure

Cosmos DB — это полностью управляемый, глобально распределенный сервис баз данных NoS'L в облаке Azure. Он был принят многими крупными компаниями по всему миру, включая Coca-Cola, Skype, ExxonMobil и Liberty Mutual.

Если ваши услуги требуют быстрого реагирования из любой точки мира, высокой доступности или эластичной масштабируемости, Cosmos DB является отличным выбором. На рисунке 5-12 показан космос DB.

![Обзор Космос DB](./media/cosmos-db-overview.png)

**Рисунок 5-12**: Обзор Космос DB

Предыдущая цифра представляет многие из встроенных облачных возможностей, доступных в Cosmos DB. В этом разделе мы рассмотрим их поближе.

### <a name="global-support"></a>Глобальная поддержка

Облачные приложения часто имеют глобальную аудиторию и требуют глобального масштаба.

Вы можете распространять базы данных Cosmos по регионам или по всему миру, размещая данные рядом с пользователями, улучшая время отклика и уменьшая задержку. Можно добавить или удалить базу данных из региона без приостановки или передислокации служб. На заднем плане Cosmos DB прозрачно реплицирует данные в каждый из настроенных регионов.

Cosmos DB поддерживает [активную/активную](https://kemptechnologies.com/white-papers/unfog-confusion-active-passive-activeactive-load-balancing/) кластеризм на глобальном уровне, позволяя настроить любой из регионов базы данных для поддержки *как записей,* так и чтения.

[Протокол Multi-Master](https://docs.microsoft.com/azure/cosmos-db/multi-master-benefits) является важной функцией в Cosmos DB, которая обеспечивает следующую функциональность:

- Неограниченная эластичная писать и читать масштабируемость.

- доступность для чтения и записи на 99,999 % по всему миру;

- гарантированные операции чтения или записи, обслуживаемые менее чем за 10 миллисекунд в 99-м процентиле.

С помощью AIS Cosmos DB [Multi-Homing](https://docs.microsoft.com/azure/cosmos-db/distribute-data-globally)ваша микрослужба автоматически знает о ближайшем регионе Azure и отправляет запросы в нее. Ближайший регион идентифицируется Cosmos DB без каких-либо изменений конфигурации. Если регион становится недоступным, функция Multi-Homing автоматически направит запросы в ближайший доступный регион.

### <a name="multi-model-support"></a>Поддержка мультимодели

При реплатформении монолитных приложений в облачную архитектуру группам разработчиков иногда приходится мигрировать с открытым исходным кодом, ноСЗЛ. Cosmos DB может помочь вам сохранить свои инвестиции в эти хранилища данных NoS'L с *помощью мультимодельной* платформы данных. На рисунке 5-13 показаны поддерживаемые [AA'L совместимости](https://www.wikiwand.com/en/Cosmos_DB)НоСЗЛ.

![Поставщики Cosmos DB](./media/cosmos-db-providers.png)

**Рисунок 5-13**: Поставщики Космос DB

Команды разработчиков могут перенести существующие базы данных Mongo, Gremlin или Cassandra в Cosmos DB с минимальными изменениями в данных или коде. Для новых приложений команды разработчиков могут выбирать среди опций с открытым исходным кодом или встроенной модели API S-L.

> Внутри компании Cosmos хранит данные в простом формате структурирования, состоящем из примитивных типов данных. Для каждого запроса движок базы данных преобразует примитивные данные в выбранное моделью представление.

В предыдущем рисунке, 5-13, обратите внимание на вариант [API таблицы.](https://docs.microsoft.com/azure/cosmos-db/table-introduction) Этот API является эволюцией хранилища таблиц Azure. Обе модели имеют одинаковую базовую таблицу, но API таблицы Cosmos DB добавляет улучшения премиум-класса, недоступные в API хранения Azure. Эти особенности контрастируют на рисунке 5-4.

![API таблиц Azure](media/azure-table-api.png)

**Рисунок 5-14**: Поставщики API таблицы Azure

Микрослужбы, потребляющие хранилище таблицы Azure, могут легко мигрировать в API таблицы таблицы Космоса. Изменения кода не требуются.

### <a name="tunable-consistency"></a>Настраиваемый согласованность

Ранее в разделе *Реляционные против НоСЗЛ* мы обсуждали тему *согласованности данных.* Согласованность данных относится к *целостности* данных. Облачные службы с распределенными данными полагаются на репликацию и должны сделать фундаментальный компромисс между согласованностью чтения, доступностью и задержкой.

Большинство распределенных баз данных позволяют разработчикам выбирать между двумя моделями согласованности: сильной согласованностью и возможной согласованностью. *Сильная консистенция* является золотым стандартом программируемости данных. Это гарантирует, что запрос всегда будет возвращать самые актуальные данные, даже если система должна иметь задержку в ожидании обновления для репликации во всех копиях базы данных. В то время как база данных, настроенная для *возможной согласованности,* немедленно вернет данные, даже если эти данные не самая актуальная копия. Последний вариант обеспечивает более высокую доступность, больший масштаб и повышенную производительность.

Azure Cosmos DB предлагает пять четко определенных [моделей согласованности,](https://docs.microsoft.com/azure/cosmos-db/consistency-levels) показанных на рисунке 5-15.

![График консистенции Cosmos DB](./media/cosmos-consistency-level-graph.png)

**Рисунок 5-15**: Уровни согласованности космоса DB

 Эти опции позволяют делать точные выборы и детальные компромиссы для согласованности, доступности и производительности данных. Рисунок 5-16 описывает каждый уровень.

![Уровни консистенции Cosmos DB](./media/cosmos-db-consistency-levels.png)

**Рисунок 5-16**: Описание уровня согласованности Космосd DB

В статье [Getting Behind 9-Ball: Космос DB Уровни согласованности Разъяснения](https://blog.jeremylikness.com/blog/2018-03-23_getting-behind-the-9ball-cosmosdb-consistency-levels/), Microsoft Облако разработчик адвокат Джереми Likeness обеспечивает отличное объяснение пяти моделей.

### <a name="partitioning"></a>Секционирование

Azure Cosmos DB включает автоматическое [разъезжание](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) для масштабирования базы данных для удовлетворения потребностей в производительности облачных служб.

Вы управляете данными в данных Cosmos DB, создавая базы данных, контейнеры и элементы.

Контейнеры живут в базе данных Cosmos DB и представляют собой схемно-агностикную группировку элементов. Элементы – это данные, которые вы добавляете в контейнер. Они представлены как документы, строки, узлы или края. Все элементы, добавленные в контейнер, автоматически индексируются.

Для раздела контейнера элементы делятся на отдельные подмножества, называемые логическими разделами. Логические разделы заполняются в зависимости от значения ключа раздела, связанного с каждым элементом в контейнере. На рисунке 5-18 показаны два контейнера с логическим разделом, основанным на значении ключа раздела.

![Механика расставания Cosmos DB](./media/cosmos-db-partitioning.png)

**Рисунок 5-18**: Механика раздела Космос DB

Примечание в предыдущей цифре, как каждый элемент включает ключ раздела либо "город" или "аэропорт". Ключ определяет логический раздел элемента. Элементы с городским кодом присваиваются контейнеру слева, а элементы с кодом аэропорта — контейнеру справа. Объединение значения ключа раздела с значением идентификатора создает индекс элемента, который однозначно идентифицирует элемент.

Внутренне, Космос DB автоматически управляет размещением [логических разделов](https://docs.microsoft.com/azure/cosmos-db/partition-data) на физических разделах для удовлетворения масштабируемости и производительности контейнера. По мере увеличения пропускной памяти и требований к хранению приложений Azure Cosmos DB перераспределяет логические разделы на большем количестве серверов. Операции перераспределения управляются Cosmos DB и вызываются без перерыва или простоя.

## <a name="newsql-databases"></a>Новые базы данных SSL

*NewS'L* — это новая технология баз данных, которая сочетает распределенную масштабируемость НоСЗЛ с гарантиями acid релятивной базы данных. Новые базы данных sS'L имеют важное значение для бизнес-систем, которые должны обрабатывать большие объемы данных, в распределенных средах, с полной поддержкой транзакций и соответствием ACID. Хотя база данных NoS'L может обеспечить масштабируемость, она не гарантирует согласованность данных. Периодические проблемы из-за несогласованных данных могут нанести бремя команде разработчиков. Разработчики должны создавать гарантии в свой код микрослужб для управления проблемами, вызванными несовместимыми данными.

Фонд облачных вычислений (CNCF) включает в себя несколько проектов по базам данных NewS'L.

| Проект | Характеристики |
| :-------- | :-------- |
| Таракан DB |Совместимая с ACID реляционная база данных, масштабирующаяся по всему миру. Добавьте новый узлы в кластер, и CockroachDB позаботится о балансировании данных в экземплярах и географических регионах. Он создает, управляет и распространяет реплики для обеспечения надежности. Это с открытым исходным кодом и в свободном доступе.  |
| TiDB | База данных с открытым исходным кодом, поддерживающая рабочие нагрузки гибридных транзакционных и аналитических процессов (HTAP). Это MyS'L-совместимо и имеет горизонтальную масштабируемость, сильную согласованность и высокую доступность.  TiDB действует как сервер MyS'L. Вы можете продолжать использовать существующие клиентские библиотеки MyS'L, не требуя значительных изменений кода в приложении. |
| YugabyteDB | Открытая исходная база данных с открытым исходным кодом, высокопроизводительная, распределенная база данных S'L. Он поддерживает низкую задержку запросов, устойчивость к сбоям и глобальное распределение данных. YugabyteDB является PostgressSS-совместимым и обрабатывает масштабирование RDBMS и интернет-масштабol. Продукт также поддерживает НоСЗЛ и совместим с Cassandra. |
|Витесс | Vitess — это решение базы данных для развертывания, масштабирования и управления большими кластерами экземпляров MyS'L. Он может работать в публичной или частной облачной архитектуре. Он сочетает в себе и расширяет многие важные функции MyS'L и имеет вертикальную и горизонтальную поддержку осколков. Основанная на YouTube, Vitess обслуживает весь трафик базы данных YouTube с 2011 года. |

Проекты с открытым исходным кодом в предыдущем рисунке доступны в Фонде облачных вычислений. Три предложения являются полноценными продуктами базы данных, которые включают поддержку .NET Core. Другая, Vitess, представляет собой систему кластеризации баз данных, которая горизонтально масштабирует большие кластеры экземпляров MyS'L.

Ключевой целью проектирования для баз данных NewS'L является работа на родном уровне в Kubernetes, пользуясь устойчивостью и масштабируемостью платформы.

Базы данных NewS'L предназначены для процветания в эфемерных облачных средах, где базовые виртуальные машины могут быть перезапущены или перенесены в любой момент. Базы данных предназначены для выживания сбоев узлов без потери данных и простоев. Например, cockroachDB способен пережить потерю машины, поддерживая три последовательные копии любых данных в кластере.

Kubernetes использует *конструкцию Службы,* позволяющую клиенту обращаться к группе идентичных процессов баз данных NewS'L из одной записи DNS. Отделяя экземпляры базы данных от адреса службы, с которой она связана, мы можем масштабироваться, не нарушая существующие экземпляры приложения. Отправка запроса в любую службу в данный момент всегда даст один и тот же результат.

В этом сценарии все экземпляры базы данных равны. Нет никаких первичных или вторичных отношений. Такие методы, как *репликация консенсуса,* найденная в CockroachDB, позволяют любому узло базы данных обрабатывать любой запрос. Если узла, получаюого запрос на сбалансированную нагрузку, имеют сяваные данные локально, он немедленно реагирует. В противном случае узел становится шлюзом и направляет запрос в соответствующие узлы, чтобы получить правильный ответ. С точки зрения клиента, каждый узлы базы данных одинаковы: они отображаются как единая *логическая* база данных с гарантиями согласованности одномашинной системы, несмотря на наличие десятков или даже сотен узлов, которые работают за кулисами.

Подробный взгляд на механику, лежащую в [основе](https://thenewstack.io/dash-four-properties-of-kubernetes-native-databases/) баз данных NewS'L, см.

## <a name="data-migration-to-the-cloud"></a>Миграция данных в облако

Одной из наиболее трудоемких задач является перенос данных с одной платформы данных на другую. Миграционная [служба данных Azure](https://azure.microsoft.com/services/database-migration/) может помочь ускорить такие усилия. Он может мигрировать данные из нескольких внешних источников базы данных на платформы Azure Data с минимальным временем простоя. Целевые платформы включают в себя следующие услуги:

- База данных SQL Azure
- База данных Azure для MySQL
- База данных Azure для MariaDB
- База данных Azure для PostgreSQL
- Cosmos DB
  
Служба предоставляет рекомендации, которые помогут вам пройти через изменения, необходимые для выполнения миграции, как малых, так и больших.

>[!div class="step-by-step"]
>[Назад](database-per-microservice.md)
>[Вперед](azure-caching.md)
