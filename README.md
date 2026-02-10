# - Assignment 2 — MapReduce Fundamentals

**Course:** High Performance Computing  
**Student:** Kabdina Kamila 
**Group:** BDA-2304 


This project implements MapReduce jobs using **Hadoop Streaming** and **Python** on the **MovieLens 100K dataset**. All jobs were executed in local (standalone) Hadoop mode on Ubuntu (WSL) with Apache Hadoop 3.3.6, Python 3, and Java OpenJDK.

Dataset files:
- `u.data` — user_id, movie_id, rating, timestamp  
- `u.item` — movie metadata (titles)

Implemented tasks:
- **Task A:** Top-20 most rated movies using `(movie_id, 1)` with a combiner to reduce shuffle traffic.
- **Task B:** Average rating per movie using `(movie_id, (rating, 1))` and computing `average = total_rating / total_count` in the reducer.
- **Task C:** Join movie IDs with movie titles using a **map-side join**, since `u.item` is small and fits in memory.
- **Bonus:** Rating distribution counting how many times each rating (1–5) appears.

Hadoop environment check:
```bash
hadoop version

Project structure:

mapreduce-movielens/
u.data
u.item
taskA_mapper.py
taskA_reducer.py
taskB_mapper.py
taskB_reducer.py
taskC_mapper.py
taskC_reducer.py
bonus_mapper.py
bonus_reducer.py
README.md

Execution commands (examples):

hadoop jar hadoop-streaming-3.3.6.jar -input u.data -output taskA_out -mapper "python3 taskA_mapper.py" -reducer "python3 taskA_reducer.py" -combiner "python3 taskA_reducer.py" -numReduceTasks 2
hadoop jar hadoop-streaming-3.3.6.jar -input u.data -output taskB_out -mapper "python3 taskB_mapper.py" -reducer "python3 taskB_reducer.py"
hadoop jar hadoop-streaming-3.3.6.jar -files u.item -input taskA_out -output taskC_out -mapper "python3 taskC_mapper.py" -reducer "python3 taskC_reducer.py"
hadoop jar hadoop-streaming-3.3.6.jar -input u.data -output bonus_out -mapper "python3 bonus_mapper.py" -reducer "python3 bonus_reducer.py"

This assignment demonstrates key MapReduce concepts including aggregation, combiners, joins, and basic performance considerations.
