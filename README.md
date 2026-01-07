# DAG Build System

## Overview
This project implements a simple build system, based on a  **Directed Acyclic Graph (DAG)**. Each node represents a task, and edges represent
dependencies between tasks.

## Features
- Load a DAG from a **JSON** file
- Parallel task execution with dependency handling
- Global resource management (CPU, RAM)
- Support for multiple concurrent builds
- Thread-safe implementation (no spin-locks)

## Constraints
- Input graph must be **acyclic**
- Cycle detection is not implemented
- Tasks communicate via files (outputs → inputs)

## Commands
- `load <path.json>` – load DAG definition
- `build <target>` – build a target or node
- `clean` – remove build artifacts
- `stats` – show system statistics
- `describe <node_id>` – show node details
- `cancel` – stop scheduling new tasks
- `exit` – shut down the system

## JSON Format
Required fields:
- `capacity` – total CPU and RAM
- `nodes` – list of tasks

Each node:
- `id` – unique identifier
- `deps` – dependencies
- `action` – `shell` or `py`
- `resources` – required CPU and RAM
- Optional: `inputs`, `outputs`

## Architecture
- Global DAG and resource registry
- One **Planner** thread per build
- Global **RafThreadPool** for execution

## Node States
**PENDING → READY → RUNNING → DONE / FAILED**
