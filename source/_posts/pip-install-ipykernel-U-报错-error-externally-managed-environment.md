---
title: 'pip install ipykernel -U 报错 error: externally-managed-environment'
date: 2024-07-28 15:03:48
tags:
    - python3
---
This error occurs because Python environment is managed by the operating system, preventing modifications to the system-wide installation using `pip`<!--more-->. To resolve this, you can create a virtual environment, which allows you to manage your own Python packages without affecting the system's Python environment. Here are the steps:

1. **Install the `python3-venv` package**:
   ```sh
   sudo apt install python3-venv
   ```

2. **Create a virtual environment**:
   ```sh
   python3 -m venv myenv
   ```

3. **Activate the virtual environment**:
   ```sh
   source myenv/bin/activate
   ```

4. **Install `ipykernel` in the virtual environment**:
   ```sh
   pip install ipykernel -U
   ```

By using a virtual environment, you can install and manage Python packages without interfering with the system's Python installation. If you need to exit the virtual environment, simply run:
```sh
deactivate
```