---
ms.openlocfilehash: ad451329d7b9ec15bc8b3c49159346d79944d692
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72393968"
---
### <a name="authentication-newtonsoftjson-types-replaced"></a><span data-ttu-id="7996b-101">Проверка подлинности. Замена типов Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="7996b-101">Authentication: Newtonsoft.Json types replaced</span></span>

<span data-ttu-id="7996b-102">В ASP.NET Core 3.0 типы `Newtonsoft.Json`, которые использовались в API проверки подлинности, заменены на типы `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="7996b-102">In ASP.NET Core 3.0, `Newtonsoft.Json` types used in Authentication APIs have been replaced with `System.Text.Json` types.</span></span> <span data-ttu-id="7996b-103">Базовое использование пакетов проверки подлинности остается неизменным, за исключением следующих случаев:</span><span class="sxs-lookup"><span data-stu-id="7996b-103">Except for the following cases, basic usage of the Authentication packages remains unaffected:</span></span>

* <span data-ttu-id="7996b-104">классы, производные от поставщиков OAuth, например от [aspnet-contrib](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers);</span><span class="sxs-lookup"><span data-stu-id="7996b-104">Classes derived from the OAuth providers, such as those from [aspnet-contrib](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers).</span></span>
* <span data-ttu-id="7996b-105">расширенные реализации манипуляций с утверждениями.</span><span class="sxs-lookup"><span data-stu-id="7996b-105">Advanced claim manipulation implementations.</span></span>

<span data-ttu-id="7996b-106">Подробную информацию см. на странице [aspnet/AspNetCore#7105](https://github.com/aspnet/AspNetCore/pull/7105).</span><span class="sxs-lookup"><span data-stu-id="7996b-106">For more information, see [aspnet/AspNetCore#7105](https://github.com/aspnet/AspNetCore/pull/7105).</span></span> <span data-ttu-id="7996b-107">Обсуждение этого вопроса см. на странице [aspnet/AspNetCore#7289](https://github.com/aspnet/AspNetCore/issues/7289).</span><span class="sxs-lookup"><span data-stu-id="7996b-107">For discussion, see [aspnet/AspNetCore#7289](https://github.com/aspnet/AspNetCore/issues/7289).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="7996b-108">Представленная версия</span><span class="sxs-lookup"><span data-stu-id="7996b-108">Version introduced</span></span>

<span data-ttu-id="7996b-109">3.0</span><span class="sxs-lookup"><span data-stu-id="7996b-109">3.0</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="7996b-110">Рекомендуемое действие</span><span class="sxs-lookup"><span data-stu-id="7996b-110">Recommended action</span></span>

<span data-ttu-id="7996b-111">В производных реализациях OAuth самым типичным вариантом является замена `JObject.Parse` на `JsonDocument.Parse` в переопределении `CreateTicketAsync`, как показано [здесь](https://github.com/aspnet/AspNetCore/pull/7105/files?utf8=%E2%9C%93&diff=unified&w=1#diff-e1c9f9740a6fe8021020a6f249c589b0L40).</span><span class="sxs-lookup"><span data-stu-id="7996b-111">For derived OAuth implementations, the most common change is to replace `JObject.Parse` with `JsonDocument.Parse` in the `CreateTicketAsync` override as shown [here](https://github.com/aspnet/AspNetCore/pull/7105/files?utf8=%E2%9C%93&diff=unified&w=1#diff-e1c9f9740a6fe8021020a6f249c589b0L40).</span></span> <span data-ttu-id="7996b-112">Объект `JsonDocument` реализует интерфейс `IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="7996b-112">`JsonDocument` implements `IDisposable`.</span></span>

<span data-ttu-id="7996b-113">В следующем списке перечислены известные изменения:</span><span class="sxs-lookup"><span data-stu-id="7996b-113">The following list outlines known changes:</span></span>

- <span data-ttu-id="7996b-114"><xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run(Newtonsoft.Json.Linq.JObject,System.Security.Claims.ClaimsIdentity,System.String)?displayProperty=nameWithType> превращается в `ClaimAction.Run(JsonElement userData, ClaimsIdentity identity, string issuer)`.</span><span class="sxs-lookup"><span data-stu-id="7996b-114"><xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run(Newtonsoft.Json.Linq.JObject,System.Security.Claims.ClaimsIdentity,System.String)?displayProperty=nameWithType> becomes `ClaimAction.Run(JsonElement userData, ClaimsIdentity identity, string issuer)`.</span></span> <span data-ttu-id="7996b-115">Аналогичным образом изменяются все производные реализации `ClaimAction`;</span><span class="sxs-lookup"><span data-stu-id="7996b-115">All derived implementations of `ClaimAction` are similarly affected.</span></span>
- <span data-ttu-id="7996b-116"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapCustomJson(Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection,System.String,System.Func{Newtonsoft.Json.Linq.JObject,System.String})?displayProperty=nameWithType> превращается в `MapCustomJson(this ClaimActionCollection collection, string claimType, Func<JsonElement, string> resolver)`;</span><span class="sxs-lookup"><span data-stu-id="7996b-116"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapCustomJson(Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection,System.String,System.Func{Newtonsoft.Json.Linq.JObject,System.String})?displayProperty=nameWithType> becomes `MapCustomJson(this ClaimActionCollection collection, string claimType, Func<JsonElement, string> resolver)`</span></span>
- <span data-ttu-id="7996b-117"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapCustomJson(Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection,System.String,System.String,System.Func{Newtonsoft.Json.Linq.JObject,System.String})?displayProperty=nameWithType> превращается в `MapCustomJson(this ClaimActionCollection collection, string claimType, string valueType, Func<JsonElement, string> resolver)`;</span><span class="sxs-lookup"><span data-stu-id="7996b-117"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapCustomJson(Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection,System.String,System.String,System.Func{Newtonsoft.Json.Linq.JObject,System.String})?displayProperty=nameWithType> becomes `MapCustomJson(this ClaimActionCollection collection, string claimType, string valueType, Func<JsonElement, string> resolver)`</span></span>
- <span data-ttu-id="7996b-118">из <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthCreatingTicketContext> удален один старый конструктор, а во втором `JObject` заменено на `JsonElement`.</span><span class="sxs-lookup"><span data-stu-id="7996b-118"><xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthCreatingTicketContext> has had one old constructor removed and the other replaced `JObject` with `JsonElement`.</span></span> <span data-ttu-id="7996b-119">Соответственно обновлены свойство `User` и метод `RunClaimActions`;</span><span class="sxs-lookup"><span data-stu-id="7996b-119">The `User` property and `RunClaimActions` method have been updated to match.</span></span>
- <span data-ttu-id="7996b-120"><xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthTokenResponse.Success(Newtonsoft.Json.Linq.JObject)> теперь принимает параметр с типом `JsonDocument` вместо `JObject`.</span><span class="sxs-lookup"><span data-stu-id="7996b-120"><xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthTokenResponse.Success(Newtonsoft.Json.Linq.JObject)> now accepts a parameter of type `JsonDocument` instead of `JObject`.</span></span> <span data-ttu-id="7996b-121">Соответственно обновлено свойство `Response`.</span><span class="sxs-lookup"><span data-stu-id="7996b-121">The `Response` property has been updated to match.</span></span> <span data-ttu-id="7996b-122">`OAuthTokenResponse` теперь является высвобождаемым, и его высвобождает `OAuthHandler`.</span><span class="sxs-lookup"><span data-stu-id="7996b-122">`OAuthTokenResponse` is now disposable and will be disposed by `OAuthHandler`.</span></span> <span data-ttu-id="7996b-123">Производные реализации OAuth, переопределяющие `ExchangeCodeAsync`, не должны высвобождать `JsonDocument` или `OAuthTokenResponse`;</span><span class="sxs-lookup"><span data-stu-id="7996b-123">Derived OAuth implementations overriding `ExchangeCodeAsync` don't need to dispose the `JsonDocument` or `OAuthTokenResponse`.</span></span>
- <span data-ttu-id="7996b-124"><xref:Microsoft.AspNetCore.Authentication.OpenIdConnect.UserInformationReceivedContext.User?displayProperty=nameWithType> стал `JsonDocument` вместо `JObject`;</span><span class="sxs-lookup"><span data-stu-id="7996b-124"><xref:Microsoft.AspNetCore.Authentication.OpenIdConnect.UserInformationReceivedContext.User?displayProperty=nameWithType> changed from `JObject` to `JsonDocument`.</span></span>
- <span data-ttu-id="7996b-125"><xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterCreatingTicketContext.User?displayProperty=nameWithType> стал `JsonElement` вместо `JObject`;</span><span class="sxs-lookup"><span data-stu-id="7996b-125"><xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterCreatingTicketContext.User?displayProperty=nameWithType> changed from `JObject` to `JsonElement`.</span></span>
- <span data-ttu-id="7996b-126"><xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterHandler.CreateTicketAsync(System.Security.Claims.ClaimsIdentity,Microsoft.AspNetCore.Authentication.AuthenticationProperties,Microsoft.AspNetCore.Authentication.Twitter.AccessToken,Newtonsoft.Json.Linq.JObject)?displayProperty=nameWithType> теперь принимает `JsonElement` вместо `JObject`.</span><span class="sxs-lookup"><span data-stu-id="7996b-126"><xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterHandler.CreateTicketAsync(System.Security.Claims.ClaimsIdentity,Microsoft.AspNetCore.Authentication.AuthenticationProperties,Microsoft.AspNetCore.Authentication.Twitter.AccessToken,Newtonsoft.Json.Linq.JObject)?displayProperty=nameWithType> changed from accepting `JObject` to `JsonElement`.</span></span>

#### <a name="category"></a><span data-ttu-id="7996b-127">Категория</span><span class="sxs-lookup"><span data-stu-id="7996b-127">Category</span></span>

<span data-ttu-id="7996b-128">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7996b-128">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="7996b-129">Затронутые API</span><span class="sxs-lookup"><span data-stu-id="7996b-129">Affected APIs</span></span>

- <xref:Microsoft.AspNetCore.Authentication.Facebook?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.Google?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.MicrosoftAccount?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.OAuth?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.OpenIdConnect?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.Twitter?displayProperty=nameWithType>

<!--

#### Affected APIs

- `N:Microsoft.AspNetCore.Authentication.Facebook`
- `N:Microsoft.AspNetCore.Authentication.Google`
- `N:Microsoft.AspNetCore.Authentication.MicrosoftAccount`
- `N:Microsoft.AspNetCore.Authentication.OAuth`
- `N:Microsoft.AspNetCore.Authentication.OpenIdConnect`
- `N:Microsoft.AspNetCore.Authentication.Twitter`

-->