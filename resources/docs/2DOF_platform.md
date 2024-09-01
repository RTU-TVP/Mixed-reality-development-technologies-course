# Мобильная платформа 2DOF

**💡Уважаемый студент!**

**Если что-то не работает или вы не знаете, что делать, обязательно читайте документацию или используйте поисковик!**

---

## Введение

**Мобильная платформа** - это конструкция, включающая кокпит, установленный на движущейся базе, которая с помощью электроприводов создает эффект перегрузок. Эта универсальная игровая платформа с сиденьем имитирует перегрузки, что делает ее идеальной для использования в авто- и авиа-симуляторах. Она отлично подходит для VR-гарнитур (Oculus, HTC, HP Reverb, Pimax и др.) или больших экранов с системами Triple Screen. Платформа функционирует как аттракцион виртуальной реальности.

Основные характеристики:

- Мобильная игровая платформа на **2-х приводах** с гоночным сиденьем.
- Поддержка авто- и авиа-симуляторов (2 в 1), с углами наклона до 15 градусов и максимальной нагрузкой до **110 кг**.
- Режимы работы: движение вперед-назад и влево-вправо.

---

## Подключение плагина и базовое использование

- **Скачайте плагин**:
  - Плагин предоставляется в формате файла с расширением `.unitypackage`.
    - Плагин доступен в [релизах на GitHub](https://github.com/RTU-TVP/Platform-With-Steering-Wheel-Plugin/releases).
- **Импортируйте плагин в проект**:
  - **Откройте Unity**.
  - **Импортируйте пакет**:
    - В меню **Assets** выберите **Import Package** > **Custom Package**.
    - Найдите и выберите скачанный `.unitypackage` файл.
    - Нажмите **Import** в появившемся окне, чтобы добавить плагин в проект.
- **Прикрепите компонент**:
  - Выберите объект, к которому нужно подключить телеметрию.
  - В инспекторе нажмите **Add Component** и найдите `CarTelemetryHandler_Sample`.
  - Прикрепите компонент `CarTelemetryHandler_Sample` к выбранному объекту.
- **Настройте ссылки**:
  - В компоненте `CarTelemetryHandler_Sample` укажите ссылки на:
    - **Transform**: Привяжите компонент `Transform` объекта для считывания его поворотов по всем осям.
    - **Rigidbody**: Привяжите компонент `Rigidbody` объекта для считывания скоростей по всем осям.

> Примечание: В Unity компоненты Transform и Rigidbody предоставляют данные о положении, поворотах и движении объекта, что важно для точного сбора телеметрии.
>

На этом базовая настройка завершена. Этот обработчик телеметрии идеально подходит для игр, где требуется отслеживание поворотов и скоростей автомобиля или другого транспортного средства.

---

## Подключение плагина и использование с кастомными значениями

### Шаги для подключения и использования с пользовательскими значениями

1. **Скачайте плагин**:
    - Плагин предоставляется в формате файла с расширением `.unitypackage`.
        - Плагин доступен в [релизах на GitHub](https://github.com/RTU-TVP/Platform-With-Steering-Wheel-Plugin/releases).
2. **Импортируйте плагин в проект**:
    - **Откройте Unity**.
    - **Импортируйте пакет**:
        - В меню **Assets** выберите **Import Package** > **Custom Package**.
        - Найдите и выберите скачанный `.unitypackage` файл.
        - Нажмите **Import** в появившемся окне.
3. **Создайте кастомный компонент для обработки телеметрии**:
    - Вместо использования `CarTelemetryHandler_Sample` создайте свой собственный класс, унаследованный от `MonoBehaviour`, для обработки специфических данных телеметрии.

### Пример кастомного компонента

```csharp
using System.Collections;
using _2DOF.Core;
using UnityEngine;

public class CustomTelemetryHandler : MonoBehaviour
{
    [SerializeField] private Transform targetTransform;  // Ссылка на Transform объекта
    [SerializeField] private Rigidbody targetRigidbody;  // Ссылка на Rigidbody объекта

    private ObjectTelemetryData _telemetryData;  // Экземпляр для хранения телеметрических данных
    private Coroutine _telemetryCoroutine;  // Корутина для обновления данных

    private void Start()
    {
        // Инициализация данных телеметрии
        _telemetryData = new ObjectTelemetryData();

        // Создание и установка экземпляра SendingData
        SendingData.Instance = new SendingData(_telemetryData);
    }

    private void OnEnable()
    {
        // Запуск корутины для обновления данных телеметрии
        _telemetryCoroutine = StartCoroutine(UpdateTelemetryData());

        // Запуск отправки данных
        SendingData.Instance.SendingStart();
    }

    private void OnDisable()
    {
        // Остановка корутины
        StopCoroutine(_telemetryCoroutine);

        // Остановка отправки данных
        SendingData.Instance.SendingStop();
    }

    private IEnumerator UpdateTelemetryData()
    {
        const float updateInterval = SendingData.WAIT_TIME / 1000f;  // Интервал обновления в секундах

        while (true)
        {
            // Проверка наличия данных телеметрии
            if (_telemetryData == null)
            {
                yield return new WaitForSeconds(updateInterval * 10f);
                continue;
            }

            // Обновление углов поворота
            if (targetTransform != null)
            {
                var rotation = targetTransform.rotation;
                _telemetryData.AnglesX = rotation.eulerAngles.x > 180 
                  ? rotation.eulerAngles.x - 360 
                 : rotation.eulerAngles.x;
                _telemetryData.AnglesZ = rotation.eulerAngles.z > 180 
                  ? rotation.eulerAngles.z - 360 
                 : rotation.eulerAngles.z;
                _telemetryData.AnglesY = rotation.eulerAngles.y > 180 
                  ? rotation.eulerAngles.y - 360 
                  : rotation.eulerAngles.y;
            }

            // Обновление скоростей
            if (targetRigidbody != null)
            {
                var velocity = targetRigidbody.velocity;
                _telemetryData.VelocityX = velocity.x;
                _telemetryData.VelocityY = velocity.y;
                _telemetryData.VelocityZ = velocity.z;
            }

            yield return new WaitForSeconds(updateInterval);
        }
    }
}
```

### Инструкции по использованию кастомного компонента

1. **Добавьте новый скрипт в Unity**:
    - В окне **Project** щелкните правой кнопкой мыши и выберите **Create** > **C# Script**.
    - Назовите скрипт, например, `CustomTelemetryHandler`.
2. **Откройте скрипт в редакторе**:
    - Скопируйте и вставьте код выше в файл `CustomTelemetryHandler.cs`.
3. **Прикрепите новый компонент к объекту**:
    - Выберите объект, к которому хотите прикрепить новый скрипт.
    - Нажмите **Add Component** и найдите `CustomTelemetryHandler`.
    - Прикрепите компонент `CustomTelemetryHandler` к выбранному объекту.
4. **Настройте ссылки**:
    - В компоненте `CustomTelemetryHandler` привяжите ссылки на:
        - **Transform**: Установите ссылку на компонент `Transform` объекта для считывания его поворотов по всем осям.
        - **Rigidbody**: Установите ссылку на компонент `Rigidbody` объекта для считывания его скоростей по всем осям.

---

## Документация

Плагин разработан для сбора и передачи телеметрических данных объекта, таких как углы поворота и скорости движения по осям. Он включает два ключевых класса:

1. **ObjectTelemetryData** - класс, который представляет телеметрические данные объекта.
2. **SendingData** - класс, отвечающий за передачу данных в отображаемый в памяти файл.

Эти классы можно использовать для мониторинга и обработки данных движения объекта в реальном времени.

### Класс ObjectTelemetryData

**Описание**

Класс **ObjectTelemetryData** используется для хранения данных телеметрии объекта, включая углы поворота и скорости движения по осям X, Y, Z.

**Свойства**

- **AnglesX**: Угол поворота объекта по оси X.
- **AnglesZ**: Угол поворота объекта по оси Z.
- **AnglesY**: Угол поворота объекта по оси Y.
- **VelocityZ**: Скорость объекта по оси Z.
- **VelocityX**: Скорость объекта по оси X.
- **VelocityY**: Скорость объекта по оси Y.
- **DataArray**: Массив, содержащий все данные телеметрии. Возвращает массив вида `[AnglesX, AnglesZ, AnglesY, VelocityZ, VelocityX, VelocityY]`.

**Методы**

- **Reset()**: Сбрасывает все данные тел

еметрии в `0`.

- **GetArrayData()**: Возвращает массив, содержащий все данные телеметрии.

### Класс SendingData

**Описание**

Класс **SendingData** отвечает за передачу данных в файл. Он считывает данные из экземпляра `ObjectTelemetryData` и отправляет их в файл, который может быть использован для анализа или мониторинга.

**Методы**

- **SendingStart()**: Начинает передачу данных.
- **SendingStop()**: Останавливает передачу данных.

### Пример использования

Пример ниже демонстрирует использование классов для сбора и передачи телеметрии:

```csharp
// Создание экземпляра ObjectTelemetryData
ObjectTelemetryData telemetryData = new ObjectTelemetryData();

// Заполнение данных
telemetryData.AnglesX = 30.0f;
telemetryData.AnglesY = 45.0f;
telemetryData.VelocityZ = 10.0f;

// Создание экземпляра SendingData и запуск передачи
SendingData sendingData = new SendingData(telemetryData);
sendingData.SendingStart();
```

**Заключение**

Эти классы предоставляют удобный интерфейс для сбора и отправки телеметрии объекта, что делает их полезными для игр и симуляторов, где требуется мониторинг движения объекта в реальном времени.
