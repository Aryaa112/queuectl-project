# queuectl-project

A CLI-Based Background Job Queue System

queuectl is a minimal, production-grade job queue system built in Python. It manages background jobs with worker processes, handles automatic retries with exponential backoff, and maintains a Dead Letter Queue (DLQ) for permanently failed jobs. All operations are managed through a simple CLI interface.

Features

Persistent Jobs: Uses SQLite (jobs.db) to ensure jobs are not lost on restart.
Multiple Workers: Supports running multiple worker processes in parallel.
Concurrency Safe: Implements atomic locking to prevent two workers from processing the same job.
Automatic Retries: Failed jobs are automatically retried with exponential backoff (base ^ attempts).
Dead Letter Queue (DLQ): Jobs that exhaust their retries are moved to a DLQ for manual inspection.
Configurable: Settings like default retry counts can be managed via the CLI.

Setup Instructions

To get queuectl running locally, follow these steps.

Clone the Repository

Clone this repository to your local machine
git clone <YOUR_REPOSITORY_URL_HERE> cd <YOUR_PROJECT_FOLDER_NAME>

Create a Virtual Environment

It's highly recommended to run this in a Python virtual environment.

On macOS/Linux
python3 -m venv venv source venv/bin/activate

On Windows
python -m venv venv .\venv\Scripts\activate

Install Dependencies

This project relies on click for the CLI interface.

pip install click

Initialize the Database

Before you can enqueue jobs, you must initialize the SQLite database. This creates the jobs.db file.

python queuectl.py initdb

Output: Database initialized successfully.

Usage Examples

queuectl is run from your terminal. The most common workflow is to have one terminal running workers and another to manage the queue.

Starting Workers

Open your first terminal and start one or more workers. They will now watch the queue for jobs.

Start 2 workers
python queuectl.py worker start --count 2

Output: Worker (PID: 12345) started. Watching for jobs... Worker (PID: 12346) started. Watching for jobs...

Enqueuing Jobs

In a second terminal, you can add jobs to the queue.

Enqueue a simple job that works
python queuectl.py enqueue "{"id":"job1",
