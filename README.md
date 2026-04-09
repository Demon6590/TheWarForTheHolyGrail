ЗАДАНИЕ.
Игра "Герои" (текстовый hack-and-slash).
Необходимо реализовать несколько разных героев, которые могут атаковать друг друга.
Необходимо реализовать возможность сохранения и восстановления состояния героя.
Необходимо реализовать текстовый интерфейс для игры.
Над балансом игры можно не думать.

Также необходимо применять паттерны и принципы SOLID, желательно сделать слоёную архитектуру приложения, желательно заложить возможность дальнейшей доработки приложения.

Для начала нужно сделать диаграммы для понимания что и как будет происходить в игре

Диаграмма действий Играков:

```mermaid
flowchart TD
    A["Запуск игры"]
    B["Выбор героя для первого игрока"]
    C["Выбор героя для второго игрока"]
    D["Начать битву"]
    E{"Мёртв ли какой-то из героев?"}
    F{"Сохранить состояние героя?(Живого)"}
    G["Добавить героя в выбор
(сохранённые герои)"]
    H["Закончить игру
(выход из приложения)"]

    A --> B
    B --> C
    C --> D
    D --> E
    E -->|Да| F
    E -->|Нет| D
    F -->|Да| G
    F -->|Нет| H
    G --> H
```

Диаграмма состояния Героя:

```mermaid
stateDiagram-v2
    [*] --> Живой
    
    state Живой {
        [*] --> Значение
        Значение --> Attacking : Attack()
        Attacking --> Значение
        
        Значение --> TakingDamage : ReceiveDamage(dmg)
        TakingDamage --> Значение : HP > 0
        TakingDamage --> Мертвый : HP <= 0
    }
    
    state Мертвый {
        [*] --> Finished  direction LR
        Finished --> [*]
    }
    
    Живой --> Мертвый
```

```mermaid
flowchart TD
    Start["Начало хода игрока"]

    ChooseAction{"Выбрать действие?"}
    Attack["Атаковать соперника"]

    CalcDamage["Рассчитать урон"]
    ApplyDamage["Нанести урон сопернику"]

    CheckEnemyHealth{"Health соперника > 0?"}
    EnemyTurn["Ход сопернику"]

    CheckPlayerAlive{"Игрок жив?"}
    DeathScreen["Игрок погиб → конец битвы"]
    ContinueBattle["Продолжить битву"]

    EndBattle["Конец битвы"]

    Start --> ChooseAction
    ChooseAction -->|"Атака"| Attack
    ChooseAction -->|"Защита"| Defend
    ChooseAction -->|"Умение"| UseAbility

    Attack --> CalcDamage
    CalcDamage --> ApplyDamage
    ApplyDamage --> CheckEnemyHealth

    CheckEnemyHealth -->|"Да"| EnemyTurn
    CheckEnemyHealth -->|"Нет"| EndBattle

    EnemyTurn -->|"Соперник атакует"| CalcPlayerDamage["Рассчитать урон по герою"]
    CalcPlayerDamage --> ApplyPlayerDamage["Нанести урон герою"]
    ApplyPlayerDamage --> CheckPlayerAlive

    CheckPlayerAlive -->|"Да"| ContinueBattle
    CheckPlayerAlive -->|"Нет"| DeathScreen

    ContinueBattle --> Start
    DeathScreen --> EndBattle
```
