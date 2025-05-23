---
tags:
  - Справочники
  - Мир
  - Окружение
  - world
  - Environment

status: new

---

# environmentParameters

На этой странице описаны глобальные параметры окружения для мира. 

Эта часть движка содержит множество тонких настроек, большинство из которых 
имеют **продвинутый технический характер**.
И обычно **не изменяются** для большинства миров.

Наиболее сложными для настройки являются параметры `speedTreeParameters`, `lensFlare` и `renderSettings`. 
Изменение этих параметров требует глубокого понимания системы рендеринга. 
Их описания ориентированы на **продвинутых пользователей**, 
которые хотят значительно изменить визуальное восприятие мира.

Для базовой настройки достаточно изменения нескольких **основных** параметров по примерам готовых миров.

## SWorldEnvironmentParameters

### vignetteTexture
- **Тип**: **Resource** [CBitmapTexture]
- **Описание**: Указывает текстуру для создания эффекта
[виньетирования](https://ru.wikipedia.org/wiki/%D0%92%D0%B8%D0%BD%D1%8C%D0%B5%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5).
(затемнения краев экрана). 

### cameraDirtTexture
- **Тип**: **Resource** [CBitmapTexture]
- **Описание**: Указывает текстуру, создающую эффект загрязнения объектива камеры. 
Особенно заметен эффект при ярком освещении.

### interiorFallbackAmbientTexture 
- **Тип**: **Resource** [CCubeTexture]
- **Описание**: Резервная кубическая текстура для фонового освещения 
интерьеров. Используется системой освещения как запасной вариант при 
отсутствии корректного освещения. (Нигде не используется в мирах CDPR)

### interiorFallbackReflectionTexture
- **Тип**: **Resource** [CCubeTexture]
- **Описание**: Резервная кубическая текстура для отражений в интерьерах.
(Нигде не используется в мирах CDPR)

### globalLightingTrajectory {.collapsed}
- **Тип**: **Class**[CGlobalLightingTrajectory]
- **Описание**: Набор параметров для конфигурации поведения глобальных источников освещения (солнца и луны) в мире.

#### yawDegrees
- **Тип**: **Float**  
- **Описание**: Базовый угол (азимут) для всех источников света в градусах. Задается в градусах.
Этот параметр определяет основное направление света и служит отправной точкой для расчета позиций солнца и луны.

#### yawDegreesSunOffset
- **Тип**: **Float** 
- **Описание**: Дополнительное смещение азимута для солнца относительно базового угла `yawDegrees`.
Позволяет дополнительно изменить азимут солнца. 

#### yawDegreesMoonOffset
- **Тип**: **Float** 
- **Описание**: Аналогично параметру `yawDegreesSunOffset`, но применяется к луне.

#### sunCurveShiftFactor
- **Тип**: **Float** 
- **Описание**: Коэффициент временного смещения солнечного цикла.
Положительные значения увеличивают продолжительность светового дня, 
смещая восход и закат на более позднее время. Отрицательные значения 
сокращают день. Применяется после расчета всех кривых, что позволяет 
корректировать время без изменения формы кривых освещения.

#### moonCurveShiftFactor
- **Тип**: **Float** 
- **Описание**: Аналогичен `sunCurveShiftFactor`, но для луны.

#### sunSqueeze 
- **Тип**: **Float**
- **Описание**: Коэффициент сжатия временного цикла солнца.
Влияет на интерполяцию времени дня, создавая более резкие или плавные изменения.

    ??? abstract "Подробнее об сжатии времени"
        Как работает "сжатие" времени?
 
        Принцип работы основан на математической формуле:

        $$
        t = (t - t_\mathrm{ref}) \cdot \text{sunSqueeze} + t_\mathrm{ref}
        $$
        
        Где:

        - t: текущее время
        - t_ref: опорная точка времени (комбинация текущего времени, значения на кривой и модификатора смещения)
        - sunSqueeze: коэффициент сжатия

        При значениях меньше 1.0 происходит растяжение цикла:

        - Рассвет и закат становятся более плавными и длительными
        - Солнце дольше находится низко над горизонтом
        - Переходы между фазами дня становятся более "мягкими"

        При значениях больше 1.0 происходит сжатие цикла:

        - Рассвет и закат становятся более резкими и быстрыми
        - Солнце проводит больше времени в зените
        - Переходы между фазами дня становятся более динамичными

#### moonSqueeze 
- **Тип**: **Float**
- **Описание**: Аналогичен `sunSequeeze`, но для луны.

#### sunHeight
- **Тип**: **Curve**
- **Описание**: Кривая, определяющая высоту солнца над горизонтом в течение 
суток. Позволяет точно настроить траекторию движения солнца.


#### moonHeight
- **Тип**: **Curve**
- **Описание**: Аналогичен `sunHeight`, но для луны.

#### lightHeight
- **Тип**: **Curve**
- **Описание**: Кривая контроля высоты главного источника освещения. 
Используется системой рендеринга для расчета направления теней и общей 
интенсивности освещения в мире.

#### lightDirChoice 
- **Тип**: **Curve**
- **Описание**: Кривая, определяющая переход между солнечным и лунным освещением.  
Значение определяет, какой источник света (солнце или луна) является основным в данный момент времени. 
Управляет плавностью смены дня и ночи, определяет интенсивность каждого источника света и время сумерек.

#### skyDayAmount
- **Тип**: **Curve**
- **Описание**: Кривая, контролирующая интенсивность дневного освещения на протяжении суток.

#### moonShaftsBeginHour
- **Тип**: **Float**
- **Описание**: Время начала появления эффекта лунного света. 
Создает едва заметное свечение вокруг луны. 
Эффект проявляется как слабое увеличение интенсивности лунного света.

#### moonShaftsEndHour
- **Тип**: **Float**
- **Описание**: Время исчезновения эффекта лунного света.

### toneMappingAdaptationSpeedUp
- **Тип**: **Float**
- **Описание**: Определяет скорость адаптации системы тонального отображения 
при переходе от темных к светлым сценам. Более высокие значения ускоряют 
адаптацию камеры к яркому свету, имитируя работу человеческого глаза.


### toneMappingAdaptationSpeedDown
- **Тип**: **Float**
- **Описание**: Контролирует скорость адаптации при переходе от светлых к 
темным сценам. Этот параметр позволяет настроить, как быстро "глаз камеры" 
привыкает к темноте.

### environmentDefinition
- **Тип**: **Resource** [CEnvironmentDefinition]
- **Описание**: Задает путь к файлу определения окружения (environment definition) 
который используется как стандартный для данного мира.
Файл содержит комплексные настройки эффектов окружения и погодных явлений. 
Env является отдельным сложным компонентом движка который будет разобран отдельно в будущем.

### scenesEnvironmentDefinition
- **Тип**: **Resource** [CEnvironmentDefinition]
- **Описание**: Задает путь к файлу определения окружения (environment definition) 
который будет использоваться по-умолчанию для сцен.

### speedTreeParameters {.collapsed}
- **Тип**: **Struct**[SGlobalSpeedTreeParameters]
- **Описание**: Набор параметров для конфигурации поведения и внешнего вида растительности в мире.

#### alphaScalar3d
- **Тип**: **Float**
- **Описание**: Глобальный множитель прозрачности для всей трехмерной 
растительности в мире.

#### alphaScalarGrassNear
- **Тип**: **Float**
- **Описание**: Множитель прозрачности для травы в ближней зоне от камеры. 
Повышение значения делает траву более прозрачной вблизи, что может помочь 
избежать визуальных артефактов и улучшить производительность.

#### alphaScalarGrass
- **Тип**: **Float**
- **Описание**: Множитель прозрачности травы на дальних дистанциях. 
Более высокие значения улучшают видимость травы вдали.

#### alphaScalarGrassDistNear
- **Тип**: **Float**
- **Описание**: Определяет минимальную дистанцию от камеры, на которой 
начинает действовать эффект прозрачности травы.

#### alphaScalarGrassDistFar
- **Тип**: **Float**
- **Описание**: Задает максимальную дистанцию применения эффекта 
прозрачности травы.

#### alphaScalarBillboards
- **Тип**: **Float**
- **Описание**: Управляет прозрачностью 2D-спрайтов растительности 
(билбордов), которые автоматически поворачиваются к камере. Влияет на то, 
насколько заметной будет упрощенная версия растительности на большом 
расстоянии.

#### billboardsGrainBias
- **Тип**: **Float**
- **Описание**: Настраивает базовое смещение текстур билбордов растительности. 
Меньшие значения делают текстуру более четкой, большие создают эффект размытия.

#### billboardsGrainAlbedoScale
- **Тип**: **Float**
- **Описание**:  Контролирует интенсивность текстурной зернистости билбордов. 
Позволяет настроить, насколько заметными будут детали текстуры.

#### billboardsGrainNormalScale
- **Тип**: **Float**
- **Описание**: Определяет масштаб карты нормалей для текстур билбордов. 
Влияет на то, как свет взаимодействует с поверхностью растительности, 
изменяя восприятие рельефности и детализации.

#### billboardsGrainClipScale
- **Тип**: **Float**
- **Описание**: Управляет масштабом отсечения текстурной зернистости, 
что влияет на визуальные эффекты при появлении и исчезновении 
растительности.

#### billboardsGrainClipBias
- **Тип**: **Float**
- **Описание**: Задает базовое смещение для отсечения текстурной 
зернистости билбордов. Влияет на то, как постепенно появляется или 
исчезает растительность при изменении дистанции до камеры.

#### billboardsGrainClipDamping
- **Тип**: **Float**
- **Описание**: Определяет степень сглаживания перехода текстурной 
зернистости билбордов при изменении расстояния от камеры. Помогает создать более 
плавные визуальные переходы.

#### grassNormalsVariation
- **Тип**: **Float**
- **Описание**: Контролирует степень случайного отклонения нормалей травы. 
Более высокие значения создают более естественное взаимодействие травы 
со светом и разнообразят её внешний вид при движении.

### weatherTemplate
- **Тип**: **Float** [C2dArray]
- **Описание**: Указывает путь к CSV-файлу, определяющему конфигурацию 
погодных условий в мире. Файл содержит настройки погодных эффектов и пути 
к .env файлам окружения. Будут разобраны отдельно в документации про env.

### disableWaterShaders
- **Тип**: **Bool**
- **Описание**: Этот параметр управляет рендерингом шейдеров воды на уровне. 
Если значение установлено в `true`, все глобальные эффекты воды на данном уровне будут отключены.
Это повысит производительность, но сильно ухудшит качество воды в мире.

### skybox {.collapsed}
- **Тип**: **Struct**[SWorldSkyboxParameters]
- **Описание**: Набор параметров настройки неба (skybox)

#### sunMesh
- **Тип**: **Resource**[CMesh]
- **Описание**: Этот параметр указывает на 3D-модель солнца, которая будет отображаться в skybox.

#### sunMaterial
- **Тип**: **Class**[CMaterialInstance]
- **Описание**: Конфигурация материала солнца из графа материала

##### Material Instance / baseMaterial
- **Тип**: **Resource**[CMaterialGraph]
- **Описание**: Устанавливает материал (**w2mg**) для 3D-модели солнца.

#### moonMesh
- **Тип**: **Resource**[CMesh]
- Этот параметр указывает на 3D-модель луны, которая будет отображаться в skybox.

#### moonMaterial
- **Тип**: **Class**[CMaterialInstance]
- **Описание**: Конфигурация материала луны из графа материала. Аналогичен `sunMaterial`

#### skyboxMesh
- **Тип**: **Resource**[CMesh]
- Этот параметр указывает на 3D-модель неба, которая будет отображаться в skybox.

#### skyboxMaterial
- **Тип**: **Class**[CMaterialInstance]
- **Описание**: Конфигурация материала неба из графа материала. Аналогичен `sunMaterial`

#### cloudsMesh
- **Тип**: **Resource**[CMesh]
- **Описание**: Этот параметр указывает на 3D-модель облаков, которая будет отображаться в skybox.

#### cloudsMaterial
- **Тип**: **Class**[CMaterialInstance]
- **Описание**: Конфигурация материала облаков из графа материала. Аналогичен `sunMaterial`

### lensFlare {.collapsed}
- **Тип**: **Struct**[SLensFlareGroupsParameters]
- **Описание**: Набор параметров отвечающих за блики объектива

#### default
- **Тип**: **Struct**[SLensFlareParameters]
- **Описание**: Параметры для бликов по-умолчанию

##### nearDistance
- **Тип**: **Float**
- **Описание**: Это значение задает минимальное расстояние, 
на котором начинают проявляться блики от солнечного света. 
Чем меньше это значение, тем ближе к камере появится эффект.

##### nearRange
- **Тип**: **Float**
- **Описание**: Этот параметр указывает диапазон эффекта на ближней дистанции, 
то есть как сильно блики будут распространяться на близком расстоянии от источника света (солнца).

##### farDistance
- **Тип**: **Float**
- **Описание**: Указывает максимальное расстояние, на котором эффект бликов от солнца может быть видим. 
Большие значения означают, что эффект будет распространяться на дальние объекты или области.

##### farRange
- **Тип**: **Float**
- **Описание**: Этот параметр контролирует диапазон бликов на дальнем расстоянии. 
Чем выше значение, тем шире будет распространяться блик на дальнем расстоянии.

##### sun
- **Тип**: **Struct**[SLensFlareParameters]
- **Описание**: Параметры бликов для солнца

##### moon
- **Тип**: **Struct**[SLensFlareParameters]
- **Описание**: Параметры бликов солнца

##### custom0 - 5
- **Тип**: **Struct**[SLensFlareParameters]
- **Описание**: Кастомные параметры бликов, не совсем ясно, где используются.

### renderSettings {.collapsed}
- **Тип**: **Struct**[SWorldRenderSettings]
- **Описание**: Набор глобальных параметров, отвечающих за рендеринг в игровом мире

#### cameraNearPlane
- **Тип**: **Float**
- **Описание**: Этот параметр определяет расстояние от камеры, при котором начинается рендеринг объектов. 
Меньшие значения позволяют камере видеть объекты, находящиеся очень близко.

#### cameraFarPlane
- **Тип**: **Float**
- **Описание**: Задает максимальное расстояние, на котором камера будет отображать объекты. 
Объекты, находящиеся за этим пределом, не будут видны.

#### ssaoBlurEnable:
- **Тип**: **Bool**
- **Описание**: Включает эффект размытия для [SSAO](https://sky.pro/wiki/gamedev/chto-takoe-ssao-i-kak-eto-rabotaet-v-igrah/)

#### ssaoNormalsEnable
- **Тип**: **Bool**
- **Описание**: Включает использование [карты нормалей ](https://habr.com/ru/articles/481480/)
для SSAO, что улучшает качество и реалистичность эффекта окклюзии.

#### envProbeSecondAmbientFilterSize
- **Тип**: **Float**
- **Описание**: Этот параметр регулирует размер фильтра для вторичного амбиентного освещения, 
применяемого на гладких поверхностях (например, кожа), улучшая их визуализацию.

#### fakeCloudsShadowSize
- **Тип**: **Float**
- **Описание**: Управляет размером тени от искусственных облаков. 
Меньшие значения создают более мелкие и детализированные тени.

#### fakeCloudsShadowSpeed
- **Тип**: **Float**
- **Описание**: Этот параметр определяет скорость движения тени искусственных облаков, 
влияя на скорость изменения освещенности.

#### fakeCloudsShadowTexture
- **Тип**: **Resource**[CTextureArray]
- **Описание**: Параметр указывающий на массив текстур для теней искусственных облаков.
Массив содержит несколько текстур для разных облаков.

#### bloomLevelsRange
- **Тип**: **UInt**[0-7]
- **Описание**: Задает количество уровней для эффекта "свечения" ([bloom](https://ru.wikipedia.org/wiki/Bloom)), 
который размывает яркие световые источники, создавая эффект свечения.

#### bloomLevelsOffset
- **Тип**: **UInt**
- **Описание**: Определяет базовое смещение для уровней эффекта bloom, влияя на то, 
как сильно будет заметен эффект свечения на разных уровнях яркости.

#### bloomScaleConst
- **Тип**: **Float**
- **Описание**: Глобальный масштабный коэффициент эффекта bloom. 
Это значение влияет на интенсивность свечения.

#### bloomDownscaleDivBase
- **Тип**: **Float**
- **Описание**: Управляет степенью уменьшения разрешения при расчете эффекта bloom. 
Влияет на производительность и качество свечения.

#### bloomDownscaleExpBase
- **Тип**: **Float**
- **Описание**: Задает экспоненциальное снижение разрешения для эффекта bloom, 
позволяя точно настроить баланс между качеством и производительностью.

#### bloomLevelScale0 - 7
- **Тип**: **Float**
- **Описание**: Параметры для точной настройки масштабирования эффекта свечения на каждом уровне яркости, 
позволяющий создать естественное распределение свечения.

#### bloomPrecision
- **Тип**: **Float**
- **Описание**: Определяет точность вычислений для эффекта bloom, влияя на качество и реалистичность 
результирующего свечения.

#### shaftsLevelIndex
- **Тип**: **UInt**
- **Описание**: Устанавливает базовый уровень интенсивности световых лучей (например, лучи света, проникающие через окна).

#### shaftsIntensity
- **Тип**: **Float**
- **Описание**: Определяет интенсивность световых лучей.

#### shaftsThresholdsScale
- **Тип**: **Float**
- **Описание**: Масштабирует пороговые значения для эффектов световых лучей, влияя на их видимость и интенсивность.

#### fresnelScaleLights
- **Тип**: **Float**
- **Описание**: Управляет масштабированием [эффекта Френеля](https://unityhub.ru/manual/StandardShaderFresnel) 
для источников света, влияя на отражения и преломления.

#### fresnelScaleEnvProbes
- **Тип**: **Float**
- **Описание**: Масштабирует эффект Френеля для окружающей среды и проб окружения, улучшая реалистичность отражений.

#### fresnelRoughnessShape
- **Тип**: **Float**
- **Описание**: Регулирует форму шероховатости эффекта Френеля, 
который используется для различных материалов (например, воды, стекла).

#### interiorDimmerAmbientLevel
- **Тип**: **Float**
- **Описание**: Определяет степень приглушения амбиентного освещения для внутренних помещений.

#### interiorVolumeSmoothExtent
- **Тип**: **Float**
- **Описание**: Контролирует масштаб сглаживания для объемных эффектов внутри помещений.

#### interiorVolumeSmoothRemovalRange
- **Тип**: **Float**
- **Описание**: Задает диапазон удаления сглаживающих эффектов в объемных данных помещений.

#### interiorVolumesFadeStartDist
- **Тип**: **Float**
- **Описание**: Определяет расстояние, с которого начинается плавное исчезновение объемных эффектов в помещениях.

#### interiorVolumesFadeRange
- **Тип**: **Float**
- **Описание**: Управляет диапазоном затухания эффектов внутри помещений.

#### interiorVolumesFadeEncodeRange
- **Тип**: **Float**
- **Описание**: Задает диапазон кодирования объемных эффектов в интерьерах.

#### distantLightStartDistance
- **Тип**: **Float**
- **Описание**: Определяет расстояние, с которого источники света начинают обрабатываться как удаленные.

#### distantLightFadeDistance
- **Тип**: **Float**
- **Описание**: Управляет дистанцией плавного перехода между обычным и удаленным освещением.

#### globalFlaresTransparencyThreshold
- **Тип**: **Float**
- **Описание**: Устанавливает пороговое значение прозрачности для глобальных бликов.

#### globalFlaresTransparencyRange
- **Тип**: **Float**
- **Описание**: Определяет диапазон изменения прозрачности глобальных бликов.

#### motionBlurSettings {.collapsed}
- **Тип**: **Struct**[SWorldMotionBlurSettings]
- **Описание**: Набор параметров для настройки эффекта размытия движения 
([motion blur](https://ru.wikipedia.org/wiki/%D0%A8%D0%B5%D0%B2%D0%B5%D0%BB%D1%91%D0%BD%D0%BA%D0%B0))

##### isPostTonemapping
- **Тип**: **Bool**
- **Описание**: Указывает, будет ли эффект размытия движения применяться после тонирования изображения.

##### distanceNear
- **Тип**: **Float**
- **Описание**: Расстояние от камеры, начиная с которого будет применяться эффект размытия для объектов, 
находящихся на ближайшем расстоянии. Меньшее значение приведет к более заметному размытию объектов, находящихся вблизи.

##### distanceRange
- **Тип**: **Float**
- **Описание**: Определяет дальность действия эффекта размытия движения.
Чем больше значение, тем дальше от камеры будет распространяться эффект размытия.

##### strengthNear
- **Тип**: **Float**
- **Описание**: Интенсивность размытия для объектов, находящихся близко к камере.

##### strengthFar
- **Тип**: **Float**
- **Описание**: Интенсивность размытия для удаленных объектов.

##### fullBlendOverPixels
- **Тип**: **Float**
- **Описание**: Количество пикселей, через которые происходит полное смешивание эффекта размытия движения. 
Чем больше значение, тем плавнее будет переход между объектами с разным уровнем размытия.

##### standoutDistanceNear
- **Тип**: **Float**
- **Описание**: Указывает на минимальное расстояние, на котором будет заметно эффект выделения объектов, 
находящихся ближе к камере.

##### standoutDistanceRange
- **Тип**: **Float**
- **Описание**: Указывает на максимальное расстояние, на котором эффект выделения объектов будет применяться.

##### standoutAmountNear
- **Тип**: **Float**
- **Описание**: Уровень выделения объектов вблизи камеры. 
Это значение определяет, насколько сильно будут выделяться объекты, находящиеся на ближайшем расстоянии.

##### standoutAmountFar
- **Тип**: **Float**
- **Описание**: Уровень выделения объектов, находящихся на дальнем расстоянии.

##### sharpenAmount
- **Тип**: **Float**
- **Описание**: Уровень повышения резкости изображения (sharpening) для размытия движения. 
Значения больше нуля делают изображение более четким, но могут добавить артефакты не краях (перешарп)

#### chromaticAberrationStart
- **Тип**: **Float**
- **Описание**: Определяет начальную точку эффекта
[хроматической аберрации.](https://radojuva.com/2011/04/chromatic-aberration/)

#### chromaticAberrationRange
- **Тип**: **Float**
- **Описание**: Задает диапазон действия хроматической аберрации. Большие значения усиливают цветовые искажения.

#### interiorFallbackReflectionThresholdLow
- **Тип**: **Float**
- **Описание**: Этот параметр управляет пороговым значением для отражений внутри помещений при использовании 
"резервных" отражений (fallback). 
Если освещенность сцены ниже заданного порога, 
то в интерьерах будет использован резервный способ рендеринга отражений.

#### interiorFallbackReflectionThresholdHigh
- **Тип**: **Float**
- **Описание**: Порог для высоких значений освещенности, при которых будут активироваться резервные отражения. 
Когда освещенность сцены достигает этого порога, отражения в интерьерах будут сильнее и более заметными. 

#### interiorFallbackReflectionBlendLow
- **Тип**: **Float**
- **Описание**: Этот параметр определяет, 
как сильно будут смешиваться (blend) резервные отражения в условиях низкой освещенности.

#### interiorFallbackReflectionBlendHigh
- **Тип**: **Float**
- **Описание**: Параметр аналогичен `interiorFallbackReflectionBlendLow`, но для высоких уровней освещенности.

#### enableEnvProbeLights
- **Тип**: **Bool**
- **Описание**: Включает использование освещения, основанного на пробах окружающей среды (envprobe).

### localWindDampers
- **Тип**: **Resource**[CResourceSimplexTree]
- **Описание**: Параметр принимает симплексное дерево на основе которого 
управляет зонами влияния ветра, регулирующими силу и направления движения воздушных масс в различных частях мира. 
Задача таких зон — моделировать влияние ветра на окружающую среду, а также влиять на погодные эффекты и физику.

### localWaterVisibility
- **Тип**: **Resource**[CResourceSimplexTree]
- **Описание**: Параметр принимает симплексное дерево на основе которого 
управляет видимостью локальной воды в различных зонах мира.

***
Автор: grandvel

*Документация поддерживается участниками сообщества [REDkit RU](https://discord.gg/kRTEy8KcNa)*
***
