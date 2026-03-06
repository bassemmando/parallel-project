# Parallel Student CSV Processing System

## Project Idea

A Java application that processes a large CSV file containing student data using **Multi-Threading**.

Instead of reading and processing student records one by one (slow), the file is split into equal parts and each part is processed by a separate thread at the same time (fast).

---

## The Problem

Processing 100,000 student records one by one takes a long time.

## The Solution

Split the data into chunks and run multiple threads in parallel — each thread handles one chunk at the same time, which makes the processing much faster.

---

                +--------------------+
                |     CSV File       |
                | 100,000 Records    |
                +---------+----------+
                          |
                          |
                          v
               +----------------------+
               |   CSV Chunk Reader   |
               +----------+-----------+
                          |
            ---------------------------------
            |        |        |        |
            v        v        v        v
        +-------+ +-------+ +-------+ +-------+
        |Thread1| |Thread2| |Thread3| |Thread4|
        |25k    | |25k    | |25k    | |25k    |
        +---+---+ +---+---+ +---+---+ +---+---+
            |         |         |         |
            ----------- Results ----------
                          |
                          v
               +----------------------+
               |  Result Aggregator   |
               +----------+-----------+
                          |
                          v
               +----------------------+
               |   Report Generator   |
               +----------+-----------+
                          |
                          v
               +----------------------+
               |     CSV Reports      |
               +----------------------+
## How It Works

**Step 1 — Generate Data**
Create a CSV file with 100,000 student records (ID, Name, Department, Subject, Grade, Semester).

**Step 2 — Read and Split**
Read the CSV file and split it into 4 equal chunks (25,000 records each).

**Step 3 — Process in Parallel**
Run 4 threads at the same time using `ExecutorService` and `Callable`.
Each thread processes its chunk and returns a result.

**Step 4 — Collect Results**
Use `Future` to collect the result from each thread, then merge everything into one final result.

**Step 5 — Generate Reports**
Save the final results into 4 CSV files:
- `summary_report.csv` — pass rate, average grade, highest and lowest grades
- `top_students.csv` — students with grade ≥ 85
- `failed_students.csv` — students with grade < 50
- `department_stats.csv` — statistics per department
 
