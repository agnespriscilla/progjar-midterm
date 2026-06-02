# TCP File Server — Network Programming Midterm Exam

> Pemrograman Jaringan (Network Programming) · ITS Surabaya · 2025

A concurrent TCP file server built from scratch in Python, supporting upload, download, list, and delete operations over a custom JSON-based protocol. Includes a full stress testing suite comparing **thread pool** vs **process pool** concurrency models across all operations.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Socket](https://img.shields.io/badge/Socket_Programming-3DDC84?style=flat)
![Threading](https://img.shields.io/badge/Multithreading-FF6B6B?style=flat)
![Multiprocessing](https://img.shields.io/badge/Multiprocessing-4B8BBE?style=flat)

---

## Overview

This project implements a file server that allows clients to transfer files over raw TCP sockets using a custom text-based protocol. Files are encoded in **Base64** for transmission, and all responses are returned as **JSON**.

Two server implementations are provided — thread pool and process pool — and their performance is benchmarked under concurrent load using a dedicated stress test client.

---

## Protocol

Communication uses a custom text-based protocol over TCP. Responses are always JSON.

| Command | Format | Response |
|---|---|---|
| List files | `LIST` | `{"status": "OK", "data": ["file1.txt", ...]}` |
| Download file | `GET <filename>` | `{"status": "OK", "data_namafile": "...", "data_file": "<base64>"}` |
| Upload file | `UPLOAD <filename> <base64_content>` | `{"status": "OK", "data": "File berhasil diupload"}` |
| Delete file | `DELETE <filename>` | `{"status": "OK", "data": "File berhasil dihapus"}` |

Files are transmitted as **Base64-encoded strings** to handle binary content safely over the text protocol.

---

## Architecture

```
Client
  │
  │  TEXT command (e.g. "GET namafile.txt")
  ▼
FileProtocol         ← parses command string, routes to correct method
  │
  ▼
FileInterface        ← executes the operation (list/get/upload/delete)
  │                     using os, base64, glob
  ▼
files/ directory     ← server-side file storage
  │
  │  JSON response ({"status": "OK", "data": ...})
  ▼
Client
```

---

## Server Implementations

| File | Concurrency Model | Description |
|---|---|---|
| `file_server_threadpool.py` | Thread pool | Fixed pool of threads handles all incoming connections |
| `file_server_processpool.py` | Process pool | Fixed pool of processes handles all incoming connections; bypasses Python GIL |

---

## Repository Structure

```
progjar-midterm/
│
├── file_interface.py                    ← File operations: list, get, upload, delete
├── file_protocol.py                     ← Protocol parser & JSON response builder
├── file_server_threadpool.py            ← Thread pool server
├── file_server_processpool.py           ← Process pool server
├── file_stress_test_client.py           ← Concurrent stress test client
│
├── files/                               ← Server-side file storage
├── downloads/                           ← Client download destination
├── test_files/                          ← Files used during stress testing
│
├── stress_test.log                      ← Full stress test log
│
├── stress_test_results_thread_download.csv   ─┐
├── stress_test_results_thread_upload.csv      │  Benchmark results:
├── stress_test_results_thread_list.csv        │  Thread pool vs Process pool
├── stress_test_results_process_download.csv   │  × Download / Upload / List
├── stress_test_results_process_upload.csv     │
└── stress_test_results_process_list.csv      ─┘
```

---

## Getting Started

### Run the thread pool server
```bash
python file_server_threadpool.py
```

### Run the process pool server
```bash
python file_server_processpool.py
```

### Run the stress test
```bash
python file_stress_test_client.py
```

Results are saved automatically to the corresponding CSV files.

---

## Stress Test Results

The project includes real benchmark data from stress testing both server types across all three operations. Results are stored in 6 CSV files covering:

- **Operations:** Download (`GET`) · Upload (`UPLOAD`) · List (`LIST`)
- **Server types:** Thread pool · Process pool
- **Metrics measured:** Response time, throughput, success/failure rate under concurrent load

The full test log is available in `stress_test.log`.

---

## Key Concepts Covered

- **TCP socket programming** — raw socket API, connection handling, pool patterns
- **Custom protocol design** — text commands with JSON responses
- **Base64 file encoding** — safe binary transfer over text protocol
- **Concurrency models** — thread pool vs process pool, tradeoffs under I/O-bound and CPU-bound load
- **Performance benchmarking** — stress testing with concurrent clients, results exported to CSV
- **Layered architecture** — clean separation of protocol parsing (`FileProtocol`) and business logic (`FileInterface`)

---

## Course Context

This is the **Midterm Exam** for the *Pemrograman Jaringan* (Network Programming) course at Institut Teknologi Sepuluh Nopember (ITS) Surabaya. The exam required building a complete file server from scratch with concurrent handling and empirical performance comparison between concurrency models.

---

## Author

**Agnes Priscilla Sekartaji Hadikusuma**  
S1 Teknik Informatika · Institut Teknologi Sepuluh Nopember (ITS) Surabaya

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/agnespriscilla)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/agnespriscilla)
[![Email](https://img.shields.io/badge/Email-D14836?style=flat&logo=gmail&logoColor=white)](mailto:agnes.priscilla33@gmail.com)
