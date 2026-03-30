# Philosophers

> *"One or more philosophers sit at a round table. There is a large bowl of spaghetti in the middle of the table."*

A 42 School project implementing the classic **Dining Philosophers Problem** using **threads** and **mutexes** in C.

---

## About

This project is an introduction to **concurrent programming** and **synchronization** in C.  
Each philosopher is represented as a **thread**, and each fork is protected by a **mutex** to prevent race conditions and deadlocks.

The simulation ends when:
- A philosopher **dies** of starvation, or
- All philosophers have eaten the required number of times *(optional)*

---

## Rules

- Philosophers sit around a round table with a fork between each of them.
- A philosopher needs **two forks** to eat (left and right).
- After eating, a philosopher puts the forks down and **sleeps**, then **thinks**.
- A philosopher **dies** if they haven't started eating within `time_to_die` milliseconds since their last meal or the start of the simulation.
- Philosophers **cannot communicate** with each other.
- They do not know if another philosopher is about to die.

---

## Usage

### Compilation

```bash
cd philo
make
```

### Execution

```bash
./philo <number_of_philosophers> <time_to_die> <time_to_eat> <time_to_sleep> [number_of_times_each_philosopher_must_eat]
```

| Argument | Description |
|---|---|
| `number_of_philosophers` | Number of philosophers (and forks) at the table |
| `time_to_die` | Time in ms before a philosopher dies if they haven't eaten |
| `time_to_eat` | Time in ms it takes to eat (holds 2 forks during this time) |
| `time_to_sleep` | Time in ms a philosopher spends sleeping |
| `number_of_times_each_philosopher_must_eat` | *(Optional)* Simulation ends when all philosophers have eaten this many times |

### Examples

```bash
# 5 philosophers, 5 should never die
./philo 5 800 200 200

# 4 philosophers, each must eat at least 3 times
./philo 4 410 200 200 3

# Edge case: 1 philosopher (should die)
./philo 1 800 200 200
```

---

## Output Format

Every state change is logged with a timestamp:

```
<timestamp_in_ms> <philosopher_id> <action>
```

Possible actions:
- `has taken a fork`
- `is eating`
- `is sleeping`
- `is thinking`
- `died`

Messages are **never interleaved** and are printed as close as possible to the actual event.

---

## Implementation Details

- Each philosopher is a **POSIX thread** (`pthread_t`)
- Each fork is a **mutex** (`pthread_mutex_t`)
- An additional **monitor thread** checks for philosopher deaths in parallel
- **Even/odd staggering** is used to avoid deadlock at startup
- Timestamps use `gettimeofday()` for precision

---

## Makefile Rules

```bash
make        # Compile the project
make clean  # Remove object files
make fclean # Remove object files and binary
make re     # Full recompile
```

---

## Concepts Covered

- POSIX Threads (`pthread`)
- Mutexes and critical sections
- Race conditions and how to avoid them
- Deadlock prevention
- Concurrent programming patterns