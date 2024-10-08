# Руль, педали Logitech G29 и коробка передач Driving Force Shifter

---

**💡Уважаемый студент!**

**Если что-то не работает или вы что-то не знаете — читайте документацию или используйте поисковик!!!**

---

## Описание

Руль Logitech G29 предназначен для полного погружения в мир виртуальных симуляторов. Он совместим с большинством игровых платформ и подключается через USB-кабель длиной 1.8 метра. Комплект включает руль диаметром 26 см, педали и необходимые крепежные элементы.

Особенности устройства:

- **Виброотдача**: Углубляет эффект погружения.
- **Угол поворота 900 градусов**: Позволяет выполнять крутые виражи.
- **Тройная педальная основа**: Надежно крепится и обеспечивает точное управление с помощью педалей ускорения, тормоза и сцепления.
- **Кожаная оплетка**: Обеспечивает комфорт и предотвращает скольжение ладоней.
- **Подрулевые лепестки**: Для переключения передач.

---

## Подключение плагина и базовое использование

- **Импортируйте asset**
  - Плагин представлен в формате `.unitypackage`.
  - Пакет доступен для скачивания в [релизах на GitHub](https://github.com/RTU-TVP/Platform-With-Steering-Wheel-Plugin/releases).
  - Импортируйте пакет в проект Unity, перетащив его в окно проекта, затем нажмите **Import** в появившемся окне.

- **Установите пакет Input System**
  - Откройте **Package Manager** в Unity.
  - Перейдите во вкладку **Unity Registry**.
  - Найдите и установите **Input System**.

- **Подключите считыватель входного контроллера (InputControllerReader)**
  - Создайте скрипт для обработки входных данных.
  - Создайте ссылку на экземпляр `InputControllerReader` в вашем скрипте:

    ```csharp
    [SerializeField] private InputControllerReader _inputControllerReader;
    ```

  - Закрепите ваш скрипт на игровом объекте в Unity:
    - Перетащите скрипт на нужный объект в иерархии.
    - В инспекторе привяжите компонент `InputControllerReader` к соответствующему полю.
  - Убедитесь, что экземпляр `InputControllerReader` привязан к вашему скрипту.

- **Пропишите логику обработки входных данных**
  - Напишите логику, которая будет обрабатывать результаты, получаемые от контроллера:

    ```csharp
    public class InputHandler : MonoBehaviour
    {
        [SerializeField] private InputControllerReader _inputControllerReader;

        private void Start()
        {
            // Пример использования обратного вызова для кнопки Home
            _inputControllerReader.OnHomeCallback += (value) => Application.Quit();

            // Пример использования обратного вызова для кнопки Options
            _inputControllerReader.OnOptionsCallback += OnOptionsCallback;
        }

        private void Update()
        {
            // Пример обработки нажатия кнопки North
            if (_inputControllerReader.NorthButton)
            {
                // Действие при нажатии кнопки North
            }

            // Пример обработки нажатия педали газа (Throttle)
            if (_inputControllerReader.Throttle > 0.5f)
            {
                // Действие при нажатии педали газа
            }
        }

        private void OnOptionsCallback(bool value)
        {
            if (value)
            {
                // Действие при нажатии кнопки Options
            }
            else
            {
                // Действие при отпускании кнопки Options
            }
        }
    }
    ```

---

## Документация

### InputControllerReader

`InputControllerReader` — это класс, предназначенный для чтения и обработки ввода от контроллера Logitech G29.

Класс предоставляет различные обработчики событий, которые вызываются при взаимодействии пользователя с контроллером, например, при нажатии кнопок или перемещении стиков. Эти обработчики обновляют соответствующие свойства класса и вызывают определенные обратные вызовы.

- **Параметры и свойства**
  - `InputControllerReader` содержит публичные свойства, представляющие состояние различных элементов контроллера, таких как кнопки и аналоговые элементы:
    - **Кнопки**: `NorthButton`, `SouthButton`, `EastButton`, `WestButton`.
    - **Аналоговые элементы**: `HatSwitch`, `Steering`, `Throttle`, `Brake`, `Clutch`, `Handbrake`.

- **События**
  - Класс включает множество публичных событий, которые срабатывают при изменении состояния контроллера. Эти события могут использоваться для реакции на действия пользователя:
    - Пример: `OnNorthButtonCallback` вызывается при нажатии кнопки "North" на контроллере.

- **Режим отладки**
  - `InputControllerReader` предоставляет методы для управления режимом отладки:
    - `SetDebugMode(bool value)`: Устанавливает режим отладки (`true` для включения, `false` для отключения).
    - `GetDebugMode()`: Возвращает текущее состояние режима отладки (включен или отключен).

  - Режим отладки помогает отслеживать действия пользователя, такие как нажатие кнопок или перемещение руля.

- **Пример использования класса**
  - Чтобы использовать `InputControllerReader`, нужно создать его экземпляр и подписаться на интересующие вас события:

    ```csharp
    public InputControllerReader inputControllerReader;
    ```

  - Пример подписки на событие нажатия кнопки "North":

    ```csharp
    inputControllerReader.OnNorthButtonCallback += value =>
    {
       if (value)
       {
           Debug.Log("North button pressed");
       }
    };
    ```

  - Чтобы включить режим отладки:

    ```csharp
    inputControllerReader.SetDebugMode(true);
    ```

  - Включив режим отладки, вы сможете видеть в консоли информацию о каждом событии ввода.

Используя класс `InputControllerReader`, вы сможете легко обрабатывать ввод от контроллера Logitech G29 в вашем приложении Unity.
