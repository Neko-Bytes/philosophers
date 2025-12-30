# Philosophers — Dining Philosophers (42 project)

A **multithreaded simulation** of Edsger Dijkstra’s classic **Dining Philosophers** problem, written in **C**.

This project focuses on:
- threads (`pthread`)
- mutex-based synchronization
- avoiding **deadlocks** and **data races**
- accurate timing + a monitor that detects philosopher death

> Repository layout note: this repo contains the mandatory implementation inside the `philo/` directory.

---

## ✅ What the program must do

- Philosophers sit around a table with **one fork between each pair**.
- Each philosopher cycles through: **eat → sleep → think**.
- To **eat**, a philosopher must hold **two forks** (left + right).
- If a philosopher doesn’t start eating within `time_to_die` ms since their last meal, they **die**.
- The simulation stops when:
  - a philosopher dies, **or**
  - every philosopher has eaten `number_of_times_each_philosopher_must_eat` times (if provided).

---

## 📦 Repository layout

```text
.
└── philo/          # mandatory version (threads + mutexes)
    ├── Makefile
    ├── includes/
    ├── src/
    └── ...
```
---

## 🧰 Requirements

* **C compiler** (clang or gcc)
* **POSIX threads** (`pthread`)

macOS and Linux are supported.

---

## 🛠️ Build

```bash
cd philo
make
```

Useful targets (if present):

```bash
make clean
make fclean
make re
```

---

## ▶️ Run

```bash
./philo <number_of_philosophers> <time_to_die> <time_to_eat> <time_to_sleep> [number_of_times_each_philosopher_must_eat]
```

### Arguments

* `number_of_philosophers` : number of philosophers (and forks)
* `time_to_die` : ms without eating before death
* `time_to_eat` : ms spent eating
* `time_to_sleep` : ms spent sleeping
* `number_of_times_each_philosopher_must_eat` *(optional)* : stop when everyone reaches this count

All times are in **milliseconds**.

---

## 🖨️ Output format

The program prints events in the usual 42 format:

```text
<timestamp_in_ms> <philo_id> <action>
```

Typical actions:

* `has taken a fork`
* `is eating`
* `is sleeping`
* `is thinking`
* `died`

---

## 🧠 Implementation notes (high level)

A typical structure used in this project:

* **One thread per philosopher**: runs the philosopher routine.
* **One monitor thread** (or a loop) checks:

  * time since last meal
  * completion condition (optional meal count)
* **Forks are mutexes**: an array of `pthread_mutex_t`.
* **Print mutex**: prevents interleaved/garbled logs.
* **Stop flag**: a shared state (protected by a mutex) tells all threads to stop cleanly.

Deadlock avoidance is commonly handled by:

* consistent fork locking order **or**
* alternating pickup order for odd/even philosophers.

---

## 🧪 Quick tests

```bash
# Should run without death for a while (example values)
./philo 5 800 200 200

# Tight death time (a philosopher will likely die)
./philo 5 310 200 200

# Must-eat condition (stops when everyone ate 7 times)
./philo 4 800 200 200 7
```

---

## 🧹 Debugging tips

### Valgrind (Linux)

```bash
valgrind --leak-check=full --show-leak-kinds=all ./philo 5 800 200 200
```

### Helgrind (race detection, Linux)

```bash
valgrind --tool=helgrind ./philo 5 800 200 200
```

> Note: race detectors can slow timing-sensitive simulations; use them to validate locking correctness.

---

## 👤 Author

* **Neko-Bytes**

---

## 📌 Notes

* This is an educational project (42 cursus).
* If you later add bonus (processes + semaphores), it’s common to include it as `philo_bonus/` and document it in a separate section.

---

