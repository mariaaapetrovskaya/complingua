### A1. Background

| Field | Value |
|-------|-------|
| **Competition Name** | Учебный проект по компьютерной лингвистике |
| **Name** | Петровская Мария |
| **Email** | mapetrovskaya@edu.hse.ru |

---

### A2. Background on you

**What is your academic/professional background?**  
Студентка учебного курса «Компьютерная лингвистика»

**Did you have any prior experience that helped you succeed?**  
Базовое знакомство с Python. Опыт работы с видео и CLIP-моделями, PyTorch, работой с датасетами на Hugging Face (в ходе проекта).

**What made you decide to enter this competition?**  
Это итоговый проект в рамках курса «Компьютерная лингвистика». Цель — разработка модели для автоматического распознавания жестов в мультимодальных корпусах.

---

### A3. Summary

В работе в том числе исследуется возможность zero-shot классификации жестов по видео с использованием модели EVA-CLIP. В качестве датасета использован набор данных «gesturedataset», содержащий 101 видео 8 типов жестов. Для извлечения кадров из видео применялась библиотека torchcodec и модель EVA02-B-16 из библиотеки open_clip. Оценка проводилась в двух режимах: на одном центральном кадре (базовый подход авторов) и на усредненных признаках всех кадров. Точность на одном кадре составила **8.9%**, что соответствует случайному угадыванию и обосновывает необходимость использования временной информации.

---

### A4. Features Selection / Engineering

**What were the most important features?**  
Модель использует кадр, он извлекается кодировщиком EVA02-B-16. Текстовые признаки — эмбеддинги строк-меток (числа от 0 до 7).

**How did you select features?**  
Признаки были предопределены архитектурой CLIP: визуальный эмбеддинг изображения и текстовый эмбеддинг названий классов. Отбор не проводился.

**Important feature transformations?**  
Все кадры проходят стандартную для CLIP предобработку: до 224×224, нормализация по среднему и стандартному отклонению ImageNet.

**External data?**  
Использовались только данные из датасета [mapetrovska/gesturedataset](https://huggingface.co/datasets/mapetrovska/gesturedataset).

---

### A5. Training Method(s)

**What training methods did you use?**  
Обучение модели не проводилось. Использовался **zero-shot inference** предобученной модели EVA02-B-16 (веса `merged2b_s8b_b131k`). Модель применялась без дообучения на целевых данных.

---

### A6. Interesting findings

- Модель EVA-CLIP, изначально обученная на изображениях, дает на видеозадачах точность на уровне случайной, если брать только один кадр (accuracy 0.089). Это подтверждает, что важен именно жест в движении.
- Основная сложность проекта — техническая адаптация: преобразование видео в кадры, работа с разными форматами (VideoDecoder и bytes), ограничения памяти в Colab.

---

### A7. Simple Features and Methods

**Is there a subset of features that would get 90-95% of your final performance?**  
В zero-shot режиме для классификации достаточно одного кадра, но качество низкое. Более эффективно использовать усреднение эмбеддингов всех кадров.

**What model that was most important?**  
Основная модель — **EVA02-B-16**.

---

### A8. Model Execution Time

| Параметр | Значение |
|----------|---------|
| **Training time (full model)** | zero-shot inference (обучение не проводилось) |
| **Inference time (full model, 101 video)** | 91 секунда |
| **Inference time per video** | Примерно 0.9 секунды на один клип |

---

### A9. References

- Модель: Sun, Q. et al. (2023). *EVA-CLIP-18B: Scaling CLIP to 18 Billion Parameters*. arXiv:2402.04252.
- Реализация модели: [open_clip](https://github.com/mlfoundations/open_clip)
- Декодер видео: [torchcodec](https://github.com/pytorch/torchcodec)
- Инструмент загрузки датасета: [Hugging Face datasets](https://huggingface.co/docs/datasets)
- Датасет жестов: [mapetrovska/gesturedataset](https://huggingface.co/datasets/mapetrovska/gesturedataset)
- Colab-ноутбук с реализацией
