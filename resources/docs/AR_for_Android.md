# Пример создания AR проекта под Mobile

**💡Уважаемый студент!**

**Если что-то не работает или вы что-то не знаете - читай документацию или гуглите!!!**

---

## Введение

Прежде, чем мы начнем, давайте разберёмся, **что такое AR?**

- В двух словах, дополненная реальность - это использование технологии для наложения любого вида визуальной или звуковой информации на реальный мир, каким мы его видим. Представьте стиль интерактивности “Железного человека".

### Что вам понадобится

- Unity Editor (в примере используется Unity 6) + Android module;
- [AR Foundation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@5.1/manual/index.html) (в примере используется 6.0.2) - фреймворк Unity для мультиплатформенной разработки AR. Предоставляет единую систему и API для взаимодействия с нашим базовым AR-движком. Он содержит все игровые объекты и классы, необходимые для быстрой разработки интерактивных AR-интерфейсов в Unity;
- [XR Interaction Toolkit](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@2.5/manual/index.html) (в примере используется 3.0.3) - высокоуровневая система взаимодействия на основе компонентов для создания возможностей XR. Она предоставляет фреймворк, который делает 3D-взаимодействия и пользовательский интерфейс доступными из событий ввода Unity;
- [ARCore](https://docs.unity3d.com/Packages/com.unity.xr.arcore@5.1/manual/index.html) (в примере используется 6.0.2) - это платформа AR для Android. AR Foundation взаимодействует с ARCore для получения функций AR на устройствах Android;
- [ARKit](https://docs.unity3d.com/Packages/com.unity.xr.arkit@5.1/manual/index.html) - это платформа AR от Apple. AR Foundation взаимодействует с ARKit, чтобы получить функции AR на устройствах Apple;
- [XR Plugin Management](https://docs.unity3d.com/Manual/com.unity.xr.management.html) представляет собой простой инструмент управления любыми плагинами XR для конкретной платформы, такими как ARKit и ARCore.

## Настройте свою среду разработки

### **Убедитесь, что ваше устройство совместимо с AR**

Возможности AR на устройствах Android основаны на ARCore, который доступен на [устройствах, поддерживаемых ARCore](https://developers.google.com/ar/devices). Убедитесь, что ваше устройство разработки совместимо с AR. В качестве альтернативы вы можете использовать правильно настроенный [AR-совместимый экземпляр Android-эмулятора](https://developers.google.com/ar/develop/java/emulator).

### **Настройте отладку по USB на своем устройстве**

Вам нужно будет включить **параметры разработчика** на вашем устройстве для запуска приложений для отладки. Если вы еще этого не сделали, обратитесь к документации Android о [включении параметров разработчика и отладке по USB](https://developer.android.com/studio/debug/dev-options#enable).

## Первичная настройка проекта

### **Создание проекта**

Создайте новый проект с шаблоном “**Universal 3D**” и назовите его по своему вкусу. Вам следует использовать Unity Hub, поскольку он значительно упрощает управление проектами.

### **Импорт SDK и инструментов**

Для наших текущих потребностей нам необходимо установить несколько дополнительных инструментов для нашего приложения, это можно сделать очень просто, найдя их в **Package Manager:**

- **AR Foundation**;
- **Google ARCore XR Plugin** (Для Android) / **Apple ARKit XR Plugin** (Для iOS);
- **OpenXR Plugin**;
- **XR Interaction Toolkit**.

Когда вы закончите, ваш менеджер пакетов должен выглядеть примерно так:

### **Измените настройки сборки**

Поскольку приложение будет работать на Android, измените платформу сборки на Android:

1. Откройте **File > Build Profiles;**
2. На панели "**Platforms**" выберите "**Android**";
3. Нажмите **Switch Platform**.

### **Настройка параметров проигрывателя**

AR Foundation необходимо настроить для инициализации XR-систем при запуске.

1. Откройте **Edit > Project Settings…** и нажмите на раздел **XR Plug-in Management;**
2. Подключаем модуль для целевой платформы:
    - Android - На вкладке **Android** включите **Google ARCore.**
    - iOS - На вкладке **iOS** включите **ARKit.**
3. На левой панели щелкните раздел "**Player**";
4. На вкладке "**Android**" в разделе "**Other Settings**" удалите **Vulkan** из **Graphics APIs.**
5. Для приложений, необходимых для AR, использующих ARCore, требуется минимальный уровень API 24. Прокрутите вниз и найдите **Minimum API Level**. Установите минимальный уровень API равным 24;
6. На вкладке "**Active Input Handler**" в разделе "**Other Settings**" выберите **Input System Package.** По умолчанию выбран вариант **Both**, этот вариант не поддерживается на Android и может вызвать проблемы с вводом и производительностью приложения.

### **Настройка рендеринга**

Для совместимости с AR Foundation в URP требуются некоторые изменения.

1. Откройте **Edit > Project Settings…** и нажмите на раздел **Graphics;**
2. В разделе **Default Render Pipeline** измените настройку с **PC** на **Mobile** (в случае, если у вас только один вариант в списке, то проигнорируйте данный пункт);
3. На панели "**Project**" перейдите через "**Assets" > "Settings"**, чтобы найти актив "**Mobile_Renderer**" в нашем случае;
4. На панели инспектора используйте **Add Renderer Feature**, чтобы добавить **AR Background Renderer Feature**. Этот компонент будет отображать изображение с камеры в вашей сцене.

## Создание AR сцены

### **Создание AR-камер и объектов сеанса AR**

Unity по умолчанию создает для нас главную камеру в сцене. Хотя ее можно было бы использовать как AR-камеру, мы заменим ее настройкой камеры AR Foundations. После этого у нас будет гораздо больше возможностей контролировать работу с AR.

Запуск любого AR-приложения называется AR-сессией, вся инициализация камеры, отслеживание и т.д. С помощью AR Foundation мы можем вручную запускать, приостанавливать или останавливать сеанс и делать многое другое.

Удалите все объекты в **Hierarchy**.

Щелкните правой кнопкой мыши в **Hierarchy** и выберите:

- **XR > AR Session**

    **AR Session** управляет жизненным циклом AR-взаимодействия. В основном это запускает AR session и обнаруживает входные данные.

- **XR > XR Origin (Mobile AR)**

    **XR Origin (Mobile AR)** преобразует AR-координаты в мировые координаты Unity, определяет центр мира AR. У него также есть дочерняя AR-камера, в которой есть сценарии, необходимые для работы в AR.

На этом мы закончили с первичной настройкой сцены.

## **Проверка проекта**

1. Убедитесь, что ваше устройство подключено и включена отладка ADB;
2. Нажмите **File > Build Profiles**;
3. В разделе **Run Device** выберите ваше устройство (если его нет в спискето скорее всего проблемы с подключением);
4. Это загрузит приложение на ваше устройство и запустит его после установки;
5. Вы должны увидеть трансляцию с камеры на экране вашего устройства.F

## Функции AR

| **Feature** | Описание | ARCore | ARKit |
| --- | --- | --- | --- |
|  |  | Android | iOS |
| [Session](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/session.html) | Включать, отключать и настраивать AR на целевой платформе. | Да | Да |
| [Device tracking](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/device-tracking.html) | Отслеживайте положение и вращение устройства в физическом пространстве. | Да | Да |
| [Camera](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/camera.html) | Визуализируйте изображения с камер устройств и выполняйте оценку освещенности. | Да | Да |
| [Plane detection](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/plane-detection.html) | Обнаружение и отслеживание плоских поверхностей. | Да | Да |
| [Image tracking](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/image-tracking.html) | Обнаружение и отслеживание 2D-изображений. | Да | Да |
| [Object tracking](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/object-tracking.html) | Обнаружение и отслеживание 3D-объектов. |  | Да |
| [Face tracking](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/face-tracking.html) | Обнаруживать и отслеживать человеческие лица. | Да | Да |
| [Body tracking](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/body-tracking.html) | Обнаруживать и отслеживать человеческое тело. |  | Да |
| [Point clouds](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/point-clouds.html) | Обнаруживать и отслеживать характерные точки. | Да | Да |
| [Raycasts](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/raycasts.html) | Направляйте лучи на отслеживаемые предметы. | Да | Да |
| [Anchors](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/anchors.html) | Отслеживание произвольных точек в пространстве. | Да | Да |
| [Meshing](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/meshing.html) | Создание сеток среды. |  | Да |
| [Environment probes](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/environment-probes.html) | Создание кубических карт среды. | Да | Да |
| [Occlusion](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/occlusion.html) | Закройте AR-контент физическими объектами и выполните сегментацию по человеку. | Да | Да |
| [Participants](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@6.0/manual/features/participant-tracking.html) | Отслеживайте другие устройства в общем сеансе AR. |  | Да |

## Продолжение разработки

### Обнаруживайте поверхности в реальном мире

Теперь, когда базовая сцена настроена, вы можете приступить к разработке игры. На этом шаге вы обнаружите плоскости и нарисуете их на сцене.

### **Добавьте `ARPlaneManager` компонент**

[`ARPlaneManager`](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@4.1/api/UnityEngine.XR.ARFoundation.ARPlaneManager.html) обнаруживает [`ARPlane`](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@4.1/api/UnityEngine.XR.ARFoundation.ARPlane.html) игровые объекты, создает их, обновляет и удаляет, когда меняется восприятие окружения устройством.

**ARPlane** - компонент, тип отслеживаемого объекта, который содержит данные, связанные с поверхностью.

1. Выберите игровой объект “**XR Origin (Mobile AR)**”. На панели инспектора нажмите ”**Add Component**”, чтобы добавить **AR Plane Manager**;
2. Настройте `ARPlaneManager`, установив `Plane Prefab` поле:
    1. Нажмите кнопку рядом с `None`, чтобы открыть окно **Выбора игрового объекта**;
    2. Выберите вкладку "**Assets**" и найдите "**AR Feathered Plane"**.
        Этот сборный материал из стартового пакета обеспечивает зернистую текстуру пола, которая будет использоваться в качестве декора плоскости.
3. Измените `Detection Mode` на `Horizontal`. Это позволяет `ARPlaneManager` создавать только горизонтальные плоскости (к примеру пол, поверхность стола и т.д.).

### **Добавьте `ARRaycastManager` компонент**

`ARRaycastManager` предоставляет функциональность raycast. На следующем шаге мы будем использовать этот объект для предоставления пользователю элементов управления.

1. Убедитесь, что вызываемый объект “**XR Origin (Mobile AR)**” выбран на панели Иерархии;
2. В инспекторе нажмите **Add Component**, чтобы добавить `ARRaycastManager` компонент к вашему игровому объекту.

Дальнейшая настройка этого компонента не требуется.

### **Запустите приложение**

1. Сделайте новую игровую сборку на ваше устройство;
2. Направьте свое устройство на горизонтальную поверхность реального мира и перемещайте его, чтобы улучшить понимание ARCore окружающего мира;
3. Когда ARCore обнаружит плоскость, вы должны увидеть текстуру из точек, покрывающую реальные поверхности. `ARPlaneManager` создает экземпляр заданной `Plane Prefab` для каждой обнаруженной плоскости.

## Проведите тест попадания по обнаруженным плоскостям

На предыдущем шаге вы запрограммировали приложение, которое может обнаруживать плоскости. Эти плоскости отражены в сцене вашей игры. Теперь вы добавите интерактивности этим плоскостям, создав прицельную сетку и автомобиль, который будет ездить по поверхности обнаруженной плоскости.

### **Создайте прицел**

Давайте сделаем так, что игрок направляет свой телефон на поверхность. Чтобы обеспечить четкую визуальную обратную связь для обозначенного местоположения, вы будете использовать прицел.

Чтобы "приклеить" его к AR-плоскости, используйте тест попадания. Тест попадания - это метод, который вычисляет пересечения при отбрасывании луча в заданном направлении. Вы будете использовать тест попадания, чтобы обнаружить пересечение в направлении обзора камеры.

### Создание прицела

Прицел должен будет располагаться на плоскости, находится в центре окна просмотра устройства.

1. Давайте создадим пустой игровой объект и назовём его **AimObject** и внутри него создадим сферу, после этого у сферы изменим масштаб по каждому вектору на “**0.1**”;
2. Создайте новый скрипт который будет называться `AimBehaviour` и поместите его **AimObject**;
3. Откройте `AimBehaviour.cs` скрипт и вставьте туда следующий код:

    ```csharp
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.XR.ARFoundation;
    using UnityEngine.XR.ARSubsystems;
    
    public class AimBehaviour : MonoBehaviour
    {
        [SerializeField] private GameObject target; // Объект, который будет показывать, что мы целимся на плоскость
        [SerializeField] private ARRaycastManager raycastManager; // Менеджер для работы с лучами
        [SerializeField] private ARPlaneManager planeManager; // Менеджер для работы с плоскостями
    
        private ARPlane _currentPlane; // Текущая плоскость, на которую мы целимся
        private Camera _mainCamera; // Основная камера
    
        private void Start()
        {
            _mainCamera = Camera.main;
        }
    
        private void Update()  
        {
            var screenCenter = _mainCamera.ViewportToScreenPoint(new Vector3(0.5f, 0.5f)); // Центр экрана
    
            var hits = new List<ARRaycastHit>();
            raycastManager.Raycast(screenCenter, hits, TrackableType.PlaneWithinBounds); // Пускаем лучи в центр экрана
            
            ARRaycastHit? hit = hits[0]; 
    
            _currentPlane = planeManager.GetPlane(hit.Value.trackableId); // Получаем плоскость, на которую мы целимся
            transform.position = hit.Value.pose.position; // Перемещаем объект, который показывает, что мы целимся на плоскость
    
            target.SetActive(_currentPlane != null); // Включаем объект, который показывает, что мы целимся на плоскость
        }
    }
    ```

4. Разберёмся, что написано в коде выше:
    1. **Поля класса**:
        - `target`: Объект, который будет использоваться для визуализации прицела на плоскости;
        - `raycastManager`: Ссылка на менеджер лучевого распознавания (`ARRaycastManager`), который используется для отправки лучей в пространство и обнаружения объектов в их пути;
        - `planeManager`: Ссылка на менеджер плоскостей (`ARPlaneManager`), который используется для управления и отслеживания плоскостей в AR-среде;
        - `_currentPlane`: Переменная для хранения ссылки на текущую плоскость, на которую направлен прицел;
        - `_mainCamera`: Ссылка на основную камеру, используемую для преобразования координат и работы с viewport.
    2. **Метод Start**: Здесь происходит инициализация переменной `_mainCamera`, которая получает ссылку на основную камеру сцены через `Camera.main`.
    3. **Метод Update**: В этом методе происходит логика определения текущей цели (плоскости) для прицела. Происходит следующее:
        - Вычисляется центр экрана относительно основной камеры;
        - Отправляются лучи из центра экрана в пространство с помощью `raycastManager.Raycast`, чтобы определить, на какой плоскости находится этот центр. Результаты сохраняются в список `hits`;
        - Из списка `hits` выбирается первый попавшийся результат (`hit`), если таковой имеется;
        - Если найдена плоскость (`_currentPlane` не равен `null`), то объект `target` перемещается в позицию, соответствующую цели, и активируется. В противном случае объект `target` деактивируется.

### **Протестируйте сетку**

1. Создадим новую игровую сборку на устройство чтобы протестировать внесенные изменения;
2. Когда вы наводите свое устройство на плоскость, вы должны видеть, как прицел следует за движениями вашей камеры.

### **Создайте наблюдателя**

Игрок будет управлять примитивом, который будет двигаться в направлении прицела.

### **Добавьте** `WatcherBehavior` **в свою сцену**

1. Откройте созданный ранее скрипт `AimBehaviour` и отредактируйте его:
    1. сделаем наш целевой объект и текущую плоскость публичными для чтения;
    2. добавим параметр для последней плоскости, на которую мы удачно целились;
    3. метод для блокировки плоскости.

    ```csharp
    using System.Collections.Generic;
    using System.Linq;
    using UnityEngine;
    using UnityEngine.XR.ARFoundation;
    using UnityEngine.XR.ARSubsystems;
    
    public class AimBehaviour : MonoBehaviour
    {
        [field: SerializeField] public GameObject target { get; private set; } // Объект, который будет показывать, что мы целимся на плоскость
        public ARPlane currentPlane { get; private set; } // Текущая плоскость, на которую мы целимся
    
        [SerializeField] private ARRaycastManager raycastManager; // Менеджер для работы с лучами
        [SerializeField] private ARPlaneManager planeManager; // Менеджер для работы с плоскостями
    
        private ARPlane _lockedPlane; // Последняя плоскость, на которую мы удачно целились
        private Camera _mainCamera; // Основная камера
    
        private void Start()
        {
            _mainCamera = Camera.main;
        }
    
        private void Update()
        {
            var screenCenter = _mainCamera.ViewportToScreenPoint(new Vector3(0.5f, 0.5f));
    
            var hits = new List<ARRaycastHit>();
            raycastManager.Raycast(screenCenter, hits, TrackableType.PlaneWithinBounds);
    
            currentPlane = null;
            ARRaycastHit? hit = null;
            if (hits.Count > 0)
            {
                var lockedPlane = _lockedPlane;
                hit = lockedPlane == null
                    ? hits[0]
                    : hits.SingleOrDefault(x => x.trackableId == lockedPlane.trackableId);
            }
    
            if (hit.HasValue)
            {
                currentPlane = planeManager.GetPlane(hit.Value.trackableId);
                transform.position = hit.Value.pose.position;
            }
    
            target.SetActive(currentPlane != null);
        }
    
        // Метод для блокировки плоскости
        public void SetLockedPlane(ARPlane keepPlane)
        {
            _lockedPlane = keepPlane;
        }
    }
    ```

2. Теперь откройте созданный ранее вами скрипт `WatcherSpawner` и отредактируйте его:

    ```csharp
    using UnityEngine;
    
    public class WatcherSpawner : MonoBehaviour
    {
        [SerializeField] private AimBehaviour aimBehaviour; // Ссылка на логику прицеливания
        [SerializeField] private GameObject watcherPrefab; // Префаб наблюдателя
        [SerializeField] private float speed = 1.2f; // Скорость наблюдателя
    
        private WatcherBehavior _watcherBehavior; // Ссылка на компонент наблюдателя
    
        // Прицеливаемся на плоскость и создаем наблюдателя 
        private void Update()
        {
            if (_watcherBehavior == null && aimBehaviour.currentPlane != null)
            {
                var instantiate = Instantiate(watcherPrefab, aimBehaviour.transform.position, Quaternion.identity);
                _watcherBehavior = instantiate.GetComponent<WatcherBehavior>();
                _watcherBehavior.SetUp(speed, aimBehaviour);
    
                aimBehaviour.SetLockedPlane(aimBehaviour.currentPlane);
            }
        }
    }
    ```

3. Разберёмся, что написано в коде выше:
    1. В начале класса определены три приватных поля с атрибутами `[SerializeField]`, которые позволяют присвоить значения этим переменным через инспектор Unity:
        - `aimBehaviour`: ссылка на компонент `AimBehaviour`, который, предположительно, управляет логикой прицеливания в игре;
        - `watcherPrefab`: префаб объекта "наблюдателя", который будет использоваться для создания новых экземпляров этих объектов в игровом мире;
        - `speed`: скорость движения объектов "наблюдателей".
    2. Приватная переменная используется для хранения ссылок на текущий объект "наблюдатель" (`_watcherBehavior`).
    3. **Метод `Update()`**: Этот метод вызывается каждый кадр и содержит основную логику работы скрипта. Он проверяет, создан ли уже объект "наблюдатель" (`_watcherBehavior == null`) и есть ли цель для прицеливания (`aimBehaviour.currentPlane!= null`). Если оба условия выполняются, то происходит следующее:
        - Создается новый экземпляр объекта "наблюдателя" с помощью метода `Instantiate`, позиционированный по координатам цели прицеливания (`aimBehaviour.transform.position`) без поворота (`Quaternion.identity`);
        - Полученный компонент `WatcherBehavior` из нового экземпляра сохраняется в `_watcherBehavior`;
        - Вызывается метод `SetUp` у компонента `WatcherBehavior`, передавая ему скорость и ссылку на `aimBehaviour` для настройки поведения объекта "наблюдателя";
        - Метод `SetLockedPlane` вызывается у `aimBehaviour`, чтобы указать, что текущая плоскость стала целью для прицеливания.
4. Теперь откройте созданный ранее вами скрипт `WatcherBehaviour` и отредактируйте его:

    ```csharp
    using UnityEngine;
    
    public class WatcherBehavior : MonoBehaviour
    {
        // Устанавливаем скорость и цель для наблюдателя
        public void SetUp(float speed, AimBehaviour aimBehaviour)
        {
            _speed = speed;
            _target = aimBehaviour.target.transform;
        }
    
        private float _speed; // Скорость наблюдателя
        private Transform _target; // Цель для наблюдателя
    
        // Поворачиваемся к цели и двигаемся к ней
        private void Update()
        {
            var trackingPosition = _target.position;
            var currentPosition = transform.position;
    
            if (Vector3.Distance(trackingPosition, currentPosition) < 0.1)
            {
                return;
            }
    
            var lookRotation = Quaternion.LookRotation(trackingPosition - currentPosition);
            transform.rotation = Quaternion.Lerp(transform.rotation, lookRotation, Time.deltaTime * 10f);
            transform.position = Vector3.MoveTowards(currentPosition, trackingPosition, _speed * Time.deltaTime);
        }
    }
    ```

5. Вот подробное описание ключевых аспектов этого класса:
    - **Публичный метод `SetUp(float speed, AimBehaviour aimBehaviour)`**: Этот метод используется для инициализации компонента `WatcherBehavior`. Он принимает два параметра: скорость (`speed`), с которой должен двигаться объект "наблюдатель", и ссылку на компонент `AimBehaviour` (`aimBehaviour`), который содержит информацию о текущей цели для наблюдения. Внутри метода эти параметры используются для установки соответствующих приватных полей `_speed` и `_target`.
    - **Приватные поля**:
        - `_speed`: хранит скорость, с которой объект "наблюдатель" будет двигаться к своей цели.
        - `_target`: хранит трансформацию (Transform) цели, к которой должен стремиться объект "наблюдатель". Эта цель устанавливается через метод `SetUp`.
    - **Метод `Update()`**: Этот метод вызывается каждый кадр и содержит логику движения и поворота объекта "наблюдателя". В нем реализована следующая логика:
        - Определяется текущая позиция объекта "наблюдателя" (`currentPosition`) и позиция цели (`trackingPosition`).
        - Если расстояние между объектом "наблюдателя" и его целью меньше некоторого порогового значения (в данном случае 0.1), то метод завершает свою работу, так как объект "наблюдатель" достиг своей цели.
        - Вычисляется поворотная матрица (`lookRotation`), которая направлена на цель, используя разницу между позицией цели и текущей позицией объекта "наблюдателя".
        - Объект "наблюдатель" плавно поворачивается к цели, используя функцию `Quaternion.Lerp` для интерполяции между текущим положением поворота и желаемым.
        - Объект "наблюдатель" также движется к цели, используя функцию `Vector3.MoveTowards` для интерполяции между его текущей позицией и позицией цели, учитывая скорость и время, прошедшее с момента последнего кадра.

### **Тест-драйв**

1. Сделайте новую игровую сборку на ваше устройство;
2. Протестируйте внесенные изменения;
3. Вы должны увидеть появляющийся примитив который будет следовать за прицелом.

## **Создание нового рендеринга плоскости**

### Создание нового материала для визуализации плоскости

Будет создан новый материал который заменит текущую визуализацию поверхности (белые точки) на свою.

1. Создайте новый материал и назовите его “**Plane Material**”;
2. Замените стандартный шейдер материала на “**FeatheredPlaneShader**” который используется для визуализации **Plane**;

3. Добавьте текстуру к материалу, именно текстура и будет отвечать за то, как будет отображаться **Plane;**
4. Можете настроить цвет по своему желанию.

### Создание префаба “**AR Plane”**

1. Создайте пустой игровой объект, назовём его “**AR Plane Prefab**”;
2. Добавьте на него следующие компоненты:
    1. ARPlane;
    2. ARPlaneMeshVisualizer;
    3. MeshCollider;
    4. MeshFilter;
    5. MeshRenderer.
3. В “**MeshRenderer**” добавьте ранее созданный материал для визуализации плоскостей;
4. Перетащите “**AR Plane Prefab**” в окно “**Prject”**, теперь у нас есть префаб и можно удалить этот объект со сцены;
5. Найдите компонент “**AR Plane Manager**” и в поле “**Plane Prefab**” замените префаб на наш.

## Пример работы с AR Raycast Manager

ARRaycastManager отвечает за проекцию лучей в "реальном" пространстве.

Давайте напишем простенький скрипт, который при тапе по экрану будет создавать примитив случайного цвета в указанных координатах объект.

1. Создайте новый скрипт и назовите его “**SpawnerMulticoloredPrimitives**”;
2. Вставьте следующий код:

    ```csharp
    using System;
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.InputSystem;
    using UnityEngine.InputSystem.Controls;
    using UnityEngine.XR.ARFoundation;
    using UnityEngine.XR.ARSubsystems;
    using Random = UnityEngine.Random;
    
    public class SpawnerMulticoloredPrimitives : MonoBehaviour
    {
        [SerializeField] private ARRaycastManager raycastManager;
        [SerializeField] private InputActionReference tapAction;
    
        private void OnEnable()
        {
            tapAction.action.Enable();
            tapAction.action.performed += OnTap;
        }
    
        private void OnDisable()
        {
            tapAction.action.Disable();
            tapAction.action.performed -= OnTap;
        }
    
        private void OnTap(InputAction.CallbackContext context)
        {
            Vector2 touchPosition;
    
            // Получение позиции касания из context
            if (context.control is Pointer inputPointer)
            {
                touchPosition = inputPointer.position.ReadValue();
            }
            else if (context.control is TouchControl pointerControl)
            {
                touchPosition = pointerControl.position.ReadValue();
            }
            else
            {
                Debug.LogWarning("Действие касания было выполнено, но оно не было сенсорным вводом.");
                touchPosition = new Vector2(Screen.width / 2f, Screen.height / 2f);
            }
    
            var arRaycastHits = new List<ARRaycastHit>();
    
            // Проверка, если касание произошло на плоскости AR
            if (!raycastManager.Raycast(touchPosition, arRaycastHits, TrackableType.PlaneWithinPolygon))
            {
                return;
            }
    
            // Получение позы первого попадания
            var hitPose = arRaycastHits[0].pose;
    
            // Выбор случайного примитива
            var values = Enum.GetValues(typeof(PrimitiveType));
            var randomPrimitive = (PrimitiveType)values.GetValue(Random.Range(0, values.Length));
    
            // Создание примитива и настройка его свойств
            var placedObject = GameObject.CreatePrimitive(randomPrimitive);
            placedObject.transform.localScale = Vector3.one * 0.1f;
            placedObject.transform.position = hitPose.position;
            placedObject.transform.rotation = hitPose.rotation;
            placedObject.GetComponent<Renderer>().material.color = Random.ColorHSV();
        }
    }
    ```

3. Этот скрипт создает случайные примитивы (например, кубы, сферы и т.д.) на поверхностях в реальном мире, которые распознаются AR камерой, при касании экрана. Разберёмся, что написано в коде выше:
    1. **Основные компоненты**
        1. `raycastManager`: Этот объект управляет лучевым обстрелом и позволяет обнаруживать плоскости в реальном мире.
        2. `tapAction`: Это ссылка на действие ввода, которое отслеживает касания экрана.
    2. **Методы жизненного цикла**
        1. **OnEnable**: Метод вызывается, когда объект активируется. Включает действие ввода и подписывается на событие `performed`, которое вызывается при выполнении действия (в данном случае, при касании экрана).
        2. **OnDisable**: Метод вызывается, когда объект деактивируется. Отключает действие ввода и отписывается от события `performed`, чтобы предотвратить вызовы метода `OnTap`, когда объект неактивен.
    3. **Обработчик касания (OnTap)**
        1. Сначала происходит получение позиции касания которая извлекается из контекста действия;
        2. После проверяется, попадает ли луч на плоскость, определенную AR системой. Если нет, метод прекращает работу. Если попадает, `arRaycastHits` содержит список попаданий, и берется первое попадание для определения позиции;
        3. Потом происходит создание примитива.  Сначала выбирается случайный тип примитива из `PrimitiveType` (например, куб, сфера и т.д.). Затем создается примитив с помощью `GameObject.CreatePrimitive`.  Примитиву задаются масштаб, позиция и ориентация в соответствии с `hitPose`. Примитиву назначается случайный цвет с помощью `Random.ColorHSV`.
4. Повесим наш скрипт на объект, к примеру “**XR Origin (Mobile AR)**” и заполним пустые поля. TapAction можно заполнить ссылкой на “**UI/Click**”.

## Подведение итогов

**Поздравляем!** Вы добрались до конца этого курса по Unity AR Foundation для Mobile.
