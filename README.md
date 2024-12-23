## Исключения в Java

### Введение

* Исключения (exceptions) – это механизм обработки ошибок в Java, возникающих во время выполнения программы (runtime).
* Исключительные ситуации могут возникать по разным причинам: ошибки ввода данных, неправильные вычисления, проблемы с доступом к файлам и т.д.

### Типы исключений

* Все исключения в Java наследуются от класса `Throwable`. Существует два основных типа исключений:
    1. **Checked Exceptions:**
       * Эти исключения компилятор проверяет во время компиляции кода. 
       * Если метод может выбросить checked exception, то он должен либо объявить это в своей сигнатуре с помощью ключевого слова `throws`, либо обработать исключение внутри блока `try-catch`.
       * Пример: `IOException` (возникает при работе с файлами).
    2. **Unchecked Exceptions (RuntimeException):**
       * Эти исключения не проверяются компилятором. 
       * Программа не обязана обрабатывать эти исключения, но может это делать по своему усмотрению.
       * Пример: `ArrayIndexOutOfBoundsException` (возникает при выходе за границы массива), `ArithmeticException` (возникает при делении на ноль).

### Обработка исключений: блок `try-catch`

* **`try { ... }`:** Внутри этого блока помещается код, который потенциально может выбросить исключение.
* **`catch (ТипИсключения имяПеременной) { ... }`:** Этот блок обрабатывает исключение определенного типа. `ТипИсключения` должен соответствовать типу ожидаемого исключения, а `имяПеременной` – это имя переменной, которая будет содержать объект исключения. Внутри блока `catch` можно выполнить действия по обработке ошибки, например, вывести сообщение об ошибке, записать информацию в лог-файл или предпринять другие действия для восстановления работы программы.
* **Порядок блоков `catch`:** Если несколько блоков `catch` ловят исключения, находящиеся в одной иерархии наследования (например, `ArithmeticException` и `RuntimeException`), то блок `catch` для потомка должен стоять перед блоком `catch` для родителя. В противном случае, блок `catch` для родителя перехватит все исключения этого типа, и блок `catch` для потомка никогда не будет выполнен.

### Блок `finally`

* **`finally { ... }`:** Этот блок выполняется всегда, независимо от того, было ли выброшено исключение или нет. Обычно в блоке `finally` выполняется закрытие ресурсов, таких как файлы или потоки ввода-вывода, чтобы предотвратить утечки ресурсов.

### Создание пользовательских исключений

* Можно создавать собственные классы исключений, расширяя `RuntimeException` или `Exception`.
* Пример: `public class ArtemException extends RuntimeException { ... }`.

### Пример использования `try-catch-finally` и собственного исключения

```java
int[] array = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

try {
    for (int i = 0; i <= array.length; i++) {
        System.out.println(array[i]);
    }
} catch (ArrayIndexOutOfBoundsException | ArithmeticException exception) {
    System.err.println("Out of array range");
} catch (Exception exception) {
    System.err.println("EXCEPTION");
}

try {
    Thread.sleep(3000);
} catch (InterruptedException e) {
    System.err.println(e.getMessage());
}

try (Scanner scanner = new Scanner(System.in);
     FileInputStream fis = new FileInputStream("some.txt")) {
    divideByZero();
} catch (ArithmeticException | ArtemException e) {
    System.err.println(e.getMessage());
} catch (Exception e) {
    System.err.println("Some issue");
} finally {
    System.out.println("FINALLY");
    scanner.close();
}


public static void divideByZero() {
    Scanner scanner = new Scanner(System.in);
    int x = scanner.nextInt();
    if (x == 0) {
        throw new ArithmeticException("Divide by zero");
    } else {
        System.out.println(1/x);
    }
    scanner.close();
}


public static void someMethod() {
    throw new ArtemException("Some issue");
}
```

### Важные моменты

* Иерархия исключений: `Throwable` -> `Exception` -> `Checked` или `Unchecked`.
* `RuntimeException` является подклассом `Exception`.
* `Error` - это критичные ошибки, с которыми невозможно работать.
* `Finally` блок выполняется всегда, независимо от результата блока `try`.
* `Try-catch` можно использовать в любом методе, где потенциально может возникнуть исключение.
* Можно создавать свои исключения, наследуясь от `RuntimeException` или `Exception`.

---------------
# Annotations

## Что такое аннотации?

* Мощный инструмент для пометки классов и методов, чтобы те выполняли определённую логику.
* Позволяют задать метаданные к элементам кода, не изменяя их основное назначение.
* Применяются к классам, полям, методам, параметрам, конструкторам, переменным.
* Позволяют проводить проверку и фильтрацию на любом этапе обработки данных, в том числе на этапе написания кода, компиляции и выполнения программы.
* Работают с помощью аннотационного процессора, который сам разработчик может создать.

## Типы аннотаций

* **Target:** Указывает цель или область действия аннотации. Определяет, где именно может быть поставлена аннотация.
* **Retention:** Определяет, насколько долго хранятся аннотации, доступные для использования. Возможные значения:
	* SOURCE - только на этапе написания кода
	* CLASS - во время компиляции и выполнения программы
	* RUNTIME - доступна на всех этапах, используется для изменения поведения класса или метода.

## Пример использования аннотации

### Задача:
Предположим, есть класс Phone с атрибутом locked и методами takePhoto и unlock().  Нужно создать аннотацию, которая будет проверять статус телефона (заблокирован или разблокирован) перед выполнением определённых действий.

### 1. Создание аннотации

```java
@Target(ElementType.TYPE)
@Retention(ElementType.RUNTIME)
public @interface IsLockePhone {
    boolean locked() default false;
}
```

### 2. Использование аннотации

```java
@IsLockePhone(locked = true)
public class Phone {
    private String name;
    private boolean locked = true; 

    public Phone(String name) {
        this.name = name;
    }

    public void takePhoto() {
        if (isLocked()) {
            System.out.println("Phone is locked!");
        } else {
            System.out.println("CLICK CLIK!");
        }
    }

    @PhoneGenerallyAvailable
    public void unlock() {
        if (!locked) {
            System.out.println("Phone unlocked successfully!");
            locked = false;
        } else {
            System.out.println("Phone already unlocked!");
        }
    }
}
```

### 3. Создание аннотационного процессора

```java
public static void annotationProcessor(Class<?> clazz) {
    boolean isLockedPhoneAnnotationPresent = clazz.isAnnotationPresent(IsLockedPhone.class);

    if (isLockedPhoneAnnotationPresent) {
        IsLockedPhone isLockedPhone = clazz.getAnnotation(IsLockedPhone.class);

        for (Method method : clazz.getDeclaredMethods()) {
            boolean isPhoneGenerallyAvailableAnnotationPresent = method.isAnnotationPresent(PhoneGenerallyAvailable.class);

            if (isPhoneGenerallyAvailableAnnotationPresent) {
                PhoneGenerallyAvailable annotation = method.getAnnotation(PhoneGenerallyAvailable.class);
                Constructor<?> constructor = clazz.getConstructor(Phone.class);
                constructor.setAccessible(true);

                method.invoke(constructor);

                System.out.println("Phone is not ready yet!");
            } else {
                System.out.println("Phone is not ready yet!");
            }
        }


        if (isLockedPhone.locked()) {
            System.out.println("Phone is fully locked!");
        } else {
            System.out.println("Phone is not present!");
        }
        
    } 
    else {
        System.out.println("Is not present!");
    }
}
```

Таким образом, данный код выполнит проверку, заблокирован ли телефон перед выполнением методов и выведет соответствующие сообщения.
