- [Введение в многопоточность](#введение-в-многопоточность)
- [Базы данных](#базы-данных)

# Введение в многопоточность
```
class EvenThread extends Thread {
    @Override
    public void run() {
        for (int i = 2; i <= 100; i += 2) {
            try {
                Thread.sleep(500); // Задержка в 500 мс
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Even: " + i);
        }
    }
}

class OddThread extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 99; i += 2) {
            try {
                Thread.sleep(500); // Задержка в 500 мс
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Odd: " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Thread evenThread = new EvenThread();
        Thread oddThread = new OddThread();

        evenThread.start(); // Запуск потока для чётных чисел
        oddThread.start();  // Запуск потока для нечётных чисел

        try {
            evenThread.join(); // Ожидание завершения потока для чётных чисел
            oddThread.join();  // Ожидание завершения потока для нечётных чисел
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread finished.");
    }
}

```

Объяснение:

В классе EvenThread и OddThread переопределён метод run(), чтобы вывести чётные и нечётные числа соответственно.
Каждый поток засыпает на 500 миллисекунд (с помощью Thread.sleep(500)), чтобы имитировать некоторую задержку.
В основном потоке (main) вызывается метод join() для каждого потока, чтобы гарантировать, что программа завершится только после завершения работы обоих потоков.
Пояснение ключевых моментов:

start() — запускает поток.

join() — заставляет основной поток ждать завершения работы другого потока перед тем, как продолжить выполнение.


# Базы данных

1. Создание таблицы books:
```
create table books(
    id SERIAL primary key,
    title VARCHAR(100) not null,
    author VARCHAR(50) not null,
    genre VARCHAR(30),
    quantity INT
);
```
2. Вставка данных в таблицу:
```
insert into books(title, author, genre, quantity)
values
('1984', 'George Orwell', 'Dystopian', 10),
('To Kill a Mockingbird', 'Harper Lee', 'Drama', 5),
('The Great Gatsby', 'F. Scott Fitzgerald', 'Tragedy', 7),
('The Catcher in the Rye', 'J.D. Salinger', 'Philosophical novel', 8),
('Moby-Dick', 'Herman Melville', 'Adventure', 4);

```
3. Запрос для получения всех книг автора "Harper Lee":
```sql
Copy code
```

4. Обновление количества экземпляров книги "1984" до 15:
```
   update books set quantity = 15 where title = '1984';
```

5. Удаление книги "Moby-Dick" из таблицы:
```
   delete from books where title = 'Moby-Dick';
```

Пояснение к решению:

- В первом шаге создаётся таблица с полями, включая автоинкремент для id, который автоматически увеличивается при добавлении новой записи.
- Во втором шаге вставляются пять известных книг с указанием авторов, жанров и количества экземпляров.
- В третьем шаге выполняется запрос для выбора книг конкретного автора (например, для поиска всех книг Харпера Ли).
- В четвёртом шаге обновляется количество экземпляров книги "1984".
- В пятом шаге удаляется книга "Moby-Dick" из базы.