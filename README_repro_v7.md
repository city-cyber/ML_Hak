# Phishing URL — V7 OOF Stack

## Файлы
- `PROJECT_phishing_v7_oof_stack_executed.ipynb` — основной notebook с EDA, очисткой, признаками, кластеризацией, baseline-моделями, полным OOF-экспериментом и финальным стеком.
- `best_phishing_model_v7.pkl` — production bundle, соответствующий `submission_v7_oof_stack.csv`.
- `submission_v7_oof_stack.csv` — файл ответа для CyberhackAI.
- `experiment_results_v7.csv` — сравнительная таблица OOF-метрик.
- `requirements_v7.txt` — версии ключевых библиотек.

## Данные
Положить рядом:
- `train.csv`
- `test.csv`
- `sample_submission.csv`

## Валидация
- Точные URL с конфликтующими метками удаляются до обучения.
- Точные дубли URL удаляются для собственной оценки.
- Базовые модели: полный `StratifiedKFold(n_splits=3, shuffle=True, random_state=42)`.
- Meta-stack: отдельная 5-fold CV на OOF-score базовых моделей.
- Threshold выбирается только по OOF/validation из `train.csv`.
- `test.csv` не используется для подбора threshold/гиперпараметров.

## Финальная подтвержденная метрика
OOF F1: **0.969350**
Precision: 0.963719
Recall: 0.975047
Accuracy: 0.970562
ROC-AUC: 0.992034

0.98 не заявляется: в выполненном leakage-free эксперименте такая F1 не достигнута.

## Финальная модель
Stack из:
1. char TF-IDF `(3,5)`, max_features=180000;
2. word TF-IDF `(1,2)`, max_features=40000;
3. SGD hinge;
4. SGD log-loss;
5. CatBoost на ручных + categorical URL-признаках;
6. LightGBM на URL-признаках;
7. LogisticRegression meta-model;
8. train-only host/root priors;
9. exact URL lookup только для однозначных train/test совпадений после модельного предсказания.

XGBoost был протестирован OOF, но в production stack не включен, так как его вклад оказался слабее.

## Запуск
1. Создать чистое окружение Python.
2. Установить зависимости из `requirements_v7.txt` (строку `python==...` при pip-установке пропустить, если используется не conda/uv).
3. Положить три CSV рядом с notebook.
4. Выполнить notebook сверху вниз.
5. Проверить финальный `submission_v7_oof_stack.csv`: 46478 строк, колонки `Id`, `Predicted`, метки только 0/1.


