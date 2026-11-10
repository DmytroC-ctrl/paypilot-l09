# paypilot-l09

## Перевірка `l09_labels.jsonl` через `loader.py`

Перевірити, що `sets/l09_labels.jsonl` завантажується `loader.py`, можна лише на гілці `L09-judge-calibration`. У `main` самого `loader.py` немає: там лише заява, `rubric.md`, набори, `judge.py`, `tools/calibrate_run.py` і звіти `reports/calibration-*.json`.

На гілці, з кореня репозиторію:

```
python3 -c "import sys; sys.path.insert(0, '.'); import loader; print(len(loader.load('sets/l09_labels.jsonl')))"
```

Очікуваний результат: `32`.
