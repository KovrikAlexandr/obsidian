пришёл лог:
    если есть свободный воркер:
        сразу начать обработку
    иначе:
        положить в очередь

воркер завершил обработку:
    если очередь не пуста:
        взять следующий лог
    иначе:
        стать свободным

Да. Это **дискретно-событийная модель** M/M/n/∞: время перескакивает от события к событию, а не идёт по маленьким шагам.

## 1. Параметры функции

```python
def simulate_mm_n(lambda_rate, mu_rate, n, simulation_time):
```

- `lambda_rate` — интенсивность прихода логов;
    
- `mu_rate` — скорость обработки одним воркером;
    
- `n` — число воркеров Fury;
    
- `simulation_time` — сколько моделируем по времени.
    

---

## 2. Текущее состояние модели

```python
current_time = 0.0
events = []
queue = []
busy_workers = 0
```

- `current_time` — текущее модельное время;
    
- `events` — будущие события;
    
- `queue` — очередь логов;
    
- `busy_workers` — сколько воркеров сейчас занято.
    

События бывают двух типов:

```python
"arrival"    # пришёл лог
"departure"  # лог обработан
```

---

## 3. Основная статистика

```python
total_arrivals = 0
total_served = 0
waited_count = 0
```

- `total_arrivals` — сколько логов пришло;
    
- `total_served` — сколько логов обработано;
    
- `waited_count` — сколько логов были вынуждены ждать.
    

Из этого считается:

```python
waiting_probability = waited_count / total_arrivals
```

---

## 4. Время ожидания и время в системе

```python
total_waiting_time = 0.0
total_system_time = 0.0
```

Для каждой обработанной заявки:

```python
waiting_time = service_start_time - arrival_time
system_time = current_time - arrival_time
```

То есть:

- `waiting_time` — сколько лог ждал в Kafka;
    
- `system_time` — сколько времени прошло от прихода до завершения обработки.
    

Потом усредняем:

```python
avg_waiting_time = total_waiting_time / total_served
avg_system_time = total_system_time / total_served
```

---

## 5. Средняя длина очереди

```python
queue_area = 0.0
last_event_time = 0.0
```

Это важный момент.

Среднюю длину очереди нельзя считать просто как среднее значение `len(queue)` по событиям. Нужно считать **среднее по времени**.

Поэтому на каждом шаге:

```python
delta = event_time - last_event_time
queue_area += len(queue) * delta
```

Смысл:

- между двумя событиями длина очереди не менялась;
    
- значит, можно посчитать площадь прямоугольника:
    

```text
длина очереди × длительность
```

В конце:

```python
avg_queue_length = queue_area / simulation_time
```

---

## 6. Вероятность простоя

```python
idle_area = 0.0
```

Если между двумя событиями:

```python
busy_workers == 0
```

значит, все воркеры свободны.

Тогда:

```python
idle_area += delta
```

В конце:

```python
idle_probability = idle_area / simulation_time
```

То есть это доля времени, когда система была пустой.

---

## 7. Коэффициент загрузки

```python
busy_area += busy_workers * delta
```

Здесь считается суммарное занятое время всех воркеров.

Пример:

- 3 воркера были заняты 2 секунды;
    
- вклад = `3 * 2 = 6 воркеро-секунд`.
    

В конце:

```python
utilization = busy_area / (n * simulation_time)
```

То есть:

```text
фактически занятое время / максимально возможное занятое время
```

---

## 8. Планирование прихода заявки

```python
def schedule_arrival(time):
    interarrival_time = random.expovariate(lambda_rate)
    heapq.heappush(events, (time + interarrival_time, "arrival", None))
```

`random.expovariate(lambda_rate)` генерирует случайное время до следующей заявки.

Это соответствует M/M/n:

- первое `M` означает пуассоновский поток;
    
- интервалы между приходами экспоненциальные.
    

Событие кладётся в `events`.

---

## 9. Планирование завершения обслуживания

```python
def schedule_departure(time, arrival_time, service_start_time):
    service_time = random.expovariate(mu_rate)
    heapq.heappush(
        events,
        (time + service_time, "departure", (arrival_time, service_start_time))
    )
```

Генерируется случайное время обслуживания.

С событием завершения мы сохраняем:

- когда заявка пришла;
    
- когда началось обслуживание.
    

Это нужно, чтобы потом посчитать:

- ожидание;
    
- полное время в системе.
    

---

## 10. Почему используется `heapq`

```python
heapq.heappush(...)
heapq.heappop(...)
```

`heapq` хранит события так, чтобы всегда доставать ближайшее по времени.

То есть модель каждый раз берёт следующее ближайшее событие:

```python
event_time, event_type, data = heapq.heappop(events)
```

---

## 11. Начало моделирования

```python
schedule_arrival(current_time)
```

Мы планируем первый приход заявки.

После этого цикл сам будет планировать следующие приходы.

---

## 12. Главный цикл

```python
while events:
    event_time, event_type, data = heapq.heappop(events)
```

На каждом шаге:

1. берём ближайшее событие;
    
2. обновляем статистику за прошедший промежуток времени;
    
3. обрабатываем событие;
    
4. создаём новые события.
    

---

## 13. Обработка прихода

```python
if event_type == "arrival":
    total_arrivals += 1
    schedule_arrival(current_time)
```

Каждый приход сразу планирует следующий приход.

Дальше:

```python
if busy_workers < n:
```

Если есть свободный воркер:

```python
busy_workers += 1
schedule_departure(...)
```

Лог сразу начинает обрабатываться.

Если все воркеры заняты:

```python
waited_count += 1
queue.append(current_time)
```

Лог попадает в очередь. В очередь кладём только время прихода — этого достаточно.

---

## 14. Обработка завершения

```python
elif event_type == "departure":
```

Лог закончил обработку.

Считаем статистику:

```python
waiting_time = service_start_time - arrival_time
system_time = current_time - arrival_time
```

Если в очереди кто-то есть:

```python
next_arrival_time = queue.pop(0)
schedule_departure(
    current_time,
    arrival_time=next_arrival_time,
    service_start_time=current_time
)
```

То есть освободившийся воркер сразу берёт следующий лог.

Если очередь пустая:

```python
busy_workers -= 1
```

Воркер становится свободным.

---

## 15. Возвращаемые результаты

```python
results = {
    "idle_probability": idle_area / simulation_time,
    "waiting_probability": waited_count / total_arrivals,
    "avg_queue_length": queue_area / simulation_time,
    "avg_waiting_time": total_waiting_time / total_served,
    "avg_system_time": total_system_time / total_served,
    "utilization": busy_area / (n * simulation_time),
}
```

Это ровно те показатели, которые требуются в лабораторной.

---

## Главная логика модели в одном фрагменте

```text
пришёл лог:
    если есть свободный воркер:
        сразу начать обработку
    иначе:
        положить в очередь

воркер завершил обработку:
    если очередь не пуста:
        взять следующий лог
    иначе:
        стать свободным
```

Это и есть модель M/M/n/∞.