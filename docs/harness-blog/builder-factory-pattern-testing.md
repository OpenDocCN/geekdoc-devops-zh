# 测试的构建工厂模式——在不破坏测试的情况下发展您的代码

> 原文：<https://www.harness.io/blog/builder-factory-pattern-testing>

生成器工厂模式有助于确保编写和维护测试比手工测试更容易。看看我们如何利用它。

为任何中型到大型代码库编写测试是至关重要的，但是每个测试都有维护成本。如果你有很难编写和维护的测试，那么开发人员会慢慢停止编写测试或者只编写最少数量的测试来合并他们的 PR。可以尝试添加严格的代码审查、代码覆盖工具等。，但这些都没有解决实际问题。事实上，你甚至可能会陷入比没有测试更糟糕的境地。

鼓励每个人编写测试的最好方法是确保编写和维护测试比手工测试更容易，即使是在短期内(当前的开发周期)。

在这篇文章中，我们将探索一种让编写和维护测试更容易的方法。我们使用 Java 作为语言，Lombok 用于生成构建器，Guice 用于依赖注入。

## **问题**

每个测试都有以下三个步骤:

1.  测试设置
2.  呼叫测试方法
3.  断言

一大挑战是测试设置。您希望您的设置易于编写、易读和可维护。

此外，一个好的代码库的特性是能够不断地重新排列和重构。但是，如果您没有一个好的方法来设置您的测试，那么一个服务/POJO 中的每一个重构或改进都会破坏许多测试的设置。这使得重构需要更大的努力，并且不太可能发生。

当您试图测试一个依赖于其他服务和其他 Java 对象(dto、beans、ORM 实体等)的服务方法时。)，为您的测试编写一个设置变得很有挑战性，因为您必须创建所有这些有效的服务和 Beans。

另一方面，如果您共享您的 Java 对象并创建助手类来创建对象，那么根据您的测试覆盖属性就变得很有挑战性。

以下四点将有助于你做到这一点:

1.理想情况下，默认情况下，对象应该是不可变的，它们应该使用 setters 来更改对象状态，只是为了在反模式中进行测试。

2.在不破坏大量现有测试的情况下，增加增量验证和演化对象的能力应该是可能的。

3.如果您使用 setters，那么发展和添加更多的检查和严格的验证就变得很有挑战性。

4.如果某些必需的属性不存在，添加新字段将会破坏您的测试。

## **构建器工厂模式**

解决上述问题的一种方法是使用构建器工厂(用默认对象创建对象的构建器),它仍然可以让您覆盖测试所需的属性。

通过共享默认对象和在测试中使用特定的逻辑来覆盖属性，这为您提供了两全其美的方法。

让我们看一个例子。我们有一个具有以下属性的类 VerifyStepTask。注意，我们有一些公共属性，比如 accountId、orgIdentifier 和 projectIdentifier，它们将在多个对象之间共享。

@Builder
@Value
公共静态类 VerifyStepTask {
String account id；
字符串组织标识符；
字符串 projectIdentifier
字符串名称；
字符串 callbackId
字符串 serviceIdentifier
字符串环境标识符；
布尔跳过；
状态状态；
公共枚举状态{进行中，完成}
}

现在，让我们定义基本的 BuilderFactory 类。

@Value
@Builder
公共静态类 Builder factory {
@ Getter @ Setter(access level。PRIVATE) Clock 时钟；
@Getter @Setter(AccessLevel。私有)上下文语境；

公共静态 builder factory get default(){
返回 BuilderFactory.builder()
。clock(clock . fixed(instant . parse(" 2020-04-22t 10:00:00Z ")，ZoneOffset。
。context(context . default context())
。build()；
}

@ Value
@ Builder
公共静态类 Context {
String account id；
字符串识别符；
字符串 projectIdentifier
字符串 serviceIdentifier
字符串环境标识符；

公共静态上下文 defaultContext() {
返回 Context.builder()
。account id(randomAlphabetic(20))
。org identifier(randomAlphabetic(20))
。project identifier(randomAlphabetic(20))
。环境标识符(randomAlphabetic(20))
。service identifier(randomAlphabetic(20))
。build()；
}
}
公共验证步骤任务。VerifyStepTaskBuilder VerifyStepTaskBuilder(){
返回 VerifyStepTask.builder()
。account id(context . getaccountid())
。org identifier(context . getorgidentifier())
。project identifier(context . getproject identifier())
。service identifier(context . getservice identifier())
。environment identifier(context . getenvidentifier())
。callback id(generate uid())
。状态(VerifyStepTask。状态.进行中)
。skip(假)；
}
}

它具有可以在多个呼叫之间共享的所有基本参数:

**时钟**–用于测试的固定时钟。

**上下文**——根据您的用例，定义可以在不同构建者之间共享的上下文。在这种情况下，由于 Context 具有 accountId、orgIdentifier 和 projectIdentifier，那么对于单个测试上下文，所有对象都将具有相同的 accountId、orgIdentifier 和 projectIdentifier。其思想是将公共共享属性放在可以在不同对象之间的构建器中使用的上下文中。

现在，让我们看看如何在实际测试中使用 BuilderFactory。

公共类 VerifyStepTaskServiceTest {
@注入私有 VerifyStepTaskService VerifyStepTaskService；
builder factory builder factory；
@Before
public void setup()抛出 IllegalAccessException {
builder factory = builder factory . get default()；
}
@ Test
@ Category(unittests . class)
public void Test create(){
String activity id = generate uid()；
verifysteptaskservice . create(
builder factory . cvngsteptaskbuilder()。activityId(活动 Id)。callbackId(activityId)。build())；
assertThat(verifysteptaskservice . get(activityId))。isNotNull()；
}
@ Test
@ Category(unit tests . class)
public void Test create _ with skip(){
String callback id = generate uid()；
verifysteptaskservice . create(builder factory . cvngsteptaskbuilder()。callbackId(回调 Id)。跳过(真)。build())；
assertThat(verifysteptaskservice . get(callbackId))。isNotNull()；
}
}

对于我们的例子来说，这是一个简单的测试，但是您可以看到测试是简洁的，并且只关注与当前测试逻辑相关的字段。但是，他们仍然可以灵活地在将来添加新的属性，或者根据额外的验证逻辑更新默认的构建器。这也适用于更复杂的场景。

假设您正在向 VerifyStepTask 添加一个新的必需属性。在这种情况下，您只需更改 verifyStepTaskBuilder 方法和所有使用 VerifyStepTask 的测试，这将获得新属性的默认值。

以下是使用这种模式的一些优点:

1.共享默认对象，这样设置就可以专注于为测试设置特定的属性。测试不需要知道当前测试中没有测试的对象的具体细节。

2.不可维护测试的一个重要原因是，当额外的验证逻辑被添加到对象中时，会破坏多个测试。现在你只需要修复你的默认构建器，而不是所有的测试。

3.您可以真正拥有 setter 自由的不可变类，因为所有的验证逻辑都可以在 builder 的构建方法(或构造函数)中

4.更容易重构，因为大多数时候你只需要改变默认的构建器对象。

5.在默认对象之间共享公共上下文，这有助于删除初始化公共属性时不必要的模板，如 accountId、projectIdentifier 等。

**需要牢记的事情**

1.仅使用 builder-factory 返回构建器，而不是实际对象。

2.没有参数化的构建器方法。这将返回默认的有效构建器对象，不需要用户的任何输入——这意味着没有参数。

3.应使用相同的构建器工厂默认对象创建相关对象。这将让您编写更简洁的测试，因为默认值是一致的和可预测的。

4.所有公共对象标识符都应该是上下文的一部分。

## **结论**

采取措施使编写单元测试变得更容易对你的设计有显著的影响。当我们开始编写更容易测试的代码时，情况也是如此。如果您还没有使用类似的东西，那么您可以尝试在代码库中使用 builder factory 模式进行测试。

有关相关主题的进一步阅读，请查看这些关于[测试智能](https://harness.io/blog/test-intelligence/)和[持续集成测试](https://harness.io/blog/continuous-integration-testing/)的文章。