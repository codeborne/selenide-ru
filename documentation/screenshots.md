---
layout: page
title :
header : Скриншоты
group: navigation
cssClass: docs
header-text: >
  <h4>Документация</h4>
  
  Скриншоты
---
{% include JB/setup %}

{% include documentation-menu.md %}

## Как сделать скриншот в тесте?

Обычно это не нужно, так как **Selenide автоматически делает скриншоты** при падении тестов. Это очень удобно для анализа ошибки.

По умолчанию Selenide складывает скриншоты в папку `build/reports/tests`.


### Можно ли сказать Selenide сохранять скриншоты в другую папку?

Да. Для этого используйте ключик `-Dselenide.reportsFolder=test-result/reports` и укажите путь к нужной папке.
Для версии 4 и ниже используйте `-Dselenide.selenide.reports=test-result/reports`
Альтернативный вариант - установить путь к скриншотам прямо в своём коде:

```java
Configuration.reportsFolder = "test-result/reports";
```


## Поддержка JUnit и TestNG

Для пользователей JUnit и TestNG мы сделали дополнительную поддержку.

### Для JUnit 4:

Чтобы автоматически делать скриншот после каждого упавшего теста:

```java
import com.codeborne.selenide.junit.ScreenShooter;

public class MyTest {
  @Rule
  public ScreenShooter makeScreenshotOnFailure = ScreenShooter.failedTests();

  // ...
}
```

В общем-то это рудимент, Selenide уже давно делает это автоматически. 

А вот чтобы автоматически делать скриншот после вообще каждого теста (в т.ч. зелёного), можно использовать следующую команду:

```java
@Rule
public ScreenShooter makeScreenshotOnFailure = ScreenShooter.failedTests().succeededTests();
```

### Для TestNG:

```java
import com.codeborne.selenide.testng.ScreenShooter;

@Listeners({ ScreenShooter.class})
public class MyTest {
  // ...
}
```

Чтобы делать скриншоты после зелёных тестов, нужно вызвать такую команду перед запуском тестов:
```java
ScreenShooter.captureSuccessfulTests = true;
```

### Для JUnit 5:

#### Как использовать в Java:

```java
  @ExtendWith({ScreenShooterExtension.class})
  public class MyTest {
    // ...
  }
```

Как использовать в (с тонкими настройками):
```java
  public class MyTest {
    @RegisterExtension
    static ScreenShooterExtension screenshotEmAll = new ScreenShooterExtension(true);
  }
```

#### Как использовать в Kotlin:
```kotlin
  @ExtendWith(ScreenShooterExtension::class)
  class MyTest {
    // ...
  }
``` 
 
Как использовать в Kotlin (с тонкими настройками):

```kotlin
  class MyTest {
    companion object {
      @JvmField
      @RegisterExtension
      val screenshotEmAll: ScreenShooterExtension = ScreenShooterExtension(true);
    }
  }
```

### В любом месте
Вы также можете сделать скриншот в любом месте теста одной строчкой:

```java
import static com.codeborne.selenide.Selenide.screenshot;

screenshot("my_file_name");
```

При этом Selenide создаст два файла: `my_file_name.png` и `my_file_name.html`
