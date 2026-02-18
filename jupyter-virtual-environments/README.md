# Using Virtual Environments as Jupyter Kernels

## A Comprehensive Guide for Data Scientists and Python Developers

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/colored.png" width="100%" />

## 📋 Table of Contents

- [Overview](#-overview)
- [The Problem](#-the-problem)
- [Solution Architecture](#-solution-architecture)
- [Prerequisites](#-prerequisites)
- [Implementation Steps](#-implementation-steps)
- [Verification and Testing](#-verification-and-testing)
- [Troubleshooting](#-troubleshooting)
- [Best Practices](#best-practices)
- [Comparison with Alternative Methods](#-comparison-with-alternative-methods)
- [Conclusion](#-conclusion)
- [Additional Resources](#-additional-resources)

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />
## 🎯 Overview

### What This Guide Covers

This guide provides a step-by-step methodology for registering Python virtual environments as Jupyter Notebook kernels without relying on Anaconda. This approach is particularly valuable for:

- **Development on resource-constrained systems** (VMs, cloud instances, low-RAM machines)
- **Production environments** requiring lightweight dependency management
- **Team collaboration** with standardized, reproducible environments
- **Project isolation** to prevent dependency conflicts

### Key Benefits

| Feature | Benefit |
|---------|---------|
| ✅ **Lightweight** | Minimal system overhead compared to Anaconda |
| ✅ **VM-Friendly** | Optimized for cloud and virtual machine deployments |
| ✅ **Industry Standard** | Uses `venv` and `pip` - the Python ecosystem standards |
| ✅ **Reproducible** | Easy to version control and share configurations |
| ✅ **Isolated** | Complete separation of project dependencies |

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## ❌ The Problem

### Common Workflow Issue

Many developers encounter this frustrating scenario:

1. **Create** a virtual environment for your project
2. **Activate** the environment in your terminal
3. **Install** all required packages (pandas, numpy, scikit-learn, etc.)
4. **Launch** Jupyter Notebook
5. **Discover** the environment is not available in Jupyter's kernel list

### Symptoms

```python
# Inside Jupyter Notebook
import pandas as pd
```

**Result:**
```
ModuleNotFoundError: No module named 'pandas'
```

Even though pandas is installed in your virtual environment!

### Root Cause

**Jupyter Notebook does not automatically detect virtual environments.**

Jupyter only recognizes registered **kernels**. A virtual environment and a Jupyter kernel are different concepts:

- **Virtual Environment**: An isolated Python installation with its own packages
- **Jupyter Kernel**: A computational engine that Jupyter uses to execute code

**The key insight:** You must explicitly register your virtual environment as a Jupyter kernel.

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## 🧩 Solution Architecture

### High-Level Process Flow

```
┌─────────────────────┐
│  Create Virtual     │
│  Environment        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Install Packages   │
│  + ipykernel        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Register as        │
│  Jupyter Kernel     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Launch Jupyter     │
│  & Select Kernel    │
└─────────────────────┘
```

### Technical Components

1. **`venv`**: Python's built-in virtual environment module
2. **`pip`**: Python package installer
3. **`ipykernel`**: IPython kernel for Jupyter
4. **Jupyter Notebook**: Interactive computing environment

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## 📋 Prerequisites

### System Requirements

| Requirement | Minimum Version | Recommended |
|-------------|-----------------|-------------|
| Python | 3.8+ | 3.10+ |
| pip | 20.0+ | Latest |
| Operating System | Windows 10 / macOS 10.14 / Ubuntu 18.04 | Any recent version |
| RAM | 2GB | 4GB+ |
| Disk Space | 500MB | 1GB+ |

### Required Tools

- **Terminal/Command Prompt**: For executing commands
- **Text Editor/IDE**: VS Code, PyCharm, or any code editor
- **Internet Connection**: For downloading packages

### Verify Your Python Installation

```bash
python --version
```

**Expected Output:**
```
Python 3.10.x
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## 💡 Implementation Steps

### Step 1: Create a Virtual Environment
![Create a Virtual Environment](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j1.png?raw=true)


**Purpose:** Create an isolated Python environment for your project.

```bash
python -m venv .venv
```

**Command Breakdown:**
- `python -m venv`: Invokes Python's virtual environment module
- `.venv`: Name of the virtual environment directory (you can change this)

**Alternative Naming Conventions:**
```bash
# Project-specific naming
python -m venv venv_project_name

# Purpose-based naming
python -m venv ml_env
python -m venv data_analysis_env
```

**Expected Result:**
A new directory `.venv` (or your chosen name) containing a complete Python installation.

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Step 2: Activate the Virtual Environment
![Activate the Virtual Environment](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j2.png?raw=true)

**Purpose:** Switch your terminal session to use the isolated Python environment.

#### Windows

```bash
.venv\Scripts\activate
```

**PowerShell Alternative:**
```powershell
.venv\Scripts\Activate.ps1
```

**Note:** If you encounter execution policy errors in PowerShell:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

#### macOS / Linux

```bash
source .venv/bin/activate
```

**Fish Shell Alternative:**
```bash
source .venv/bin/activate.fish
```

**Verification:**

Your terminal prompt should now display the environment name:

```bash
(.venv) user@machine:~/project$
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />
### Step 3: Upgrade pip and Install Core Packages

**Purpose:** Ensure you have the latest package installer and install project dependencies.

#### Upgrade pip

```bash
pip install --upgrade pip
```

**Why upgrade?**
- Access to latest package versions
- Security patches
- Bug fixes
- Performance improvements

#### Install Project Dependencies
![Activate the Virtual Environment](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j3.png?raw=true)

```bash
# Data science stack example
pip install pandas numpy matplotlib seaborn scikit-learn

# Or install from requirements file
pip install -r requirements.txt
```

**Best Practice:** Create a `requirements.txt` for reproducibility:
```bash
pip freeze > requirements.txt
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Step 4: Install Jupyter and IPython Kernel
![Install IPython Kernel](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j6.png?raw=true)

**Purpose:** Install the necessary packages to create a Jupyter kernel from your virtual environment.

```bash
pip install jupyter ipykernel
```

**Package Roles:**
- **`jupyter`**: Jupyter Notebook interface and server
- **`ipykernel`**: Python kernel for Jupyter that enables environment registration

**Verification:**
```bash
jupyter --version
python -m ipykernel --version
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Step 5: Register Virtual Environment as Jupyter Kernel
![Register Virtual Environment as Jupyter Kernel](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j7.png?raw=true)

**Purpose:** Make your virtual environment available as a selectable kernel in Jupyter.

```bash
python -m ipykernel install --user --name .venv --display-name "Python (.venv)"
```

**Command Explanation:**

| Parameter | Purpose | Example Value |
|-----------|---------|---------------|
| `python -m ipykernel install` | Invokes kernel installation | - |
| `--user` | Install for current user only (no admin rights needed) | - |
| `--name .venv` | Internal kernel identifier | `.venv`, `ml_env`, `project_env` |
| `--display-name "Python (.venv)"` | Name shown in Jupyter UI | `"Python (ML)"`, `"Data Analysis"` |

**Best Practice Naming:**
```bash
# Descriptive naming for multiple projects
python -m ipykernel install --user --name sales_analysis --display-name "Python (Sales Analysis)"
python -m ipykernel install --user --name ml_model --display-name "Python (ML Model Dev)"
```

**Expected Output:**
```
Installed kernelspec .venv in /home/user/.local/share/jupyter/kernels/.venv
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Step 6: Launch Jupyter Notebook
![Launch Jupyter Notebook](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j8.png?raw=true)

**Purpose:** Start the Jupyter Notebook server.

```bash
jupyter notebook
```

**What Happens:**
![Launched Jupyter Notebook](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j9.png?raw=true)
1. Jupyter server starts on `localhost:8888` (or next available port)
2. Your default web browser opens automatically
3. File browser shows your current directory

**Alternative Launch Options:**

```bash
# Launch in a specific directory
jupyter notebook /path/to/project

# Launch without opening browser
jupyter notebook --no-browser

# Specify port
jupyter notebook --port 8889
```

**Access Jupyter:**
If the browser doesn't open automatically, navigate to:
```
http://localhost:8888
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Step 7: Select Virtual Environment Kernel
![Select Virtual Environment Kernel](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j10.png?raw=true)
![Select Virtual Environment Kernel](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j11.png?raw=true)
![Select Virtual Environment Kernel](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j13.png?raw=true)
![Select Virtual Environment Kernel](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j14.png?raw=true)
![Select Virtual Environment Kernel](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j15.png?raw=true)
![Select Virtual Environment Kernel](https://github.com/coder-akram-khan/data-stack-guides/blob/main/assets/images/j16.png?raw=true)

**Purpose:** Configure your notebook to use the registered virtual environment kernel.

#### For New Notebooks

1. Click **"New"** dropdown in top-right corner
2. Select **"Python (.venv)"** from the list

#### For Existing Notebooks

1. Open your notebook
2. Navigate to: **Kernel → Change Kernel**
3. Select **"Python (.venv)"** from the dropdown menu

**Visual Confirmation:**

The kernel name appears in the top-right corner of the notebook interface:
```
Python (.venv) ● Connected
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## ☑ Verification and Testing

### Test 1: Verify Kernel Path

**Purpose:** Confirm Jupyter is using the correct Python interpreter.

Run this in a notebook cell:

```python
import sys
print(sys.executable)
```

**Expected Output:**

**Windows:**
```
C:\Users\YourName\project\.venv\Scripts\python.exe
```

**macOS/Linux:**
```
/home/username/project/.venv/bin/python
```

**✅ Success Indicator:** Path contains your virtual environment directory name (`.venv`)

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Test 2: Verify Package Installation

**Purpose:** Confirm installed packages are accessible.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

print(f"Pandas version: {pd.__version__}")
print(f"NumPy version: {np.__version__}")
```

**Expected Output:**
```
Pandas version: 2.1.0
NumPy version: 1.24.3
```

**If ModuleNotFoundError occurs:**
- Verify kernel selection (Step 7)
- Reinstall packages in virtual environment
- Re-register kernel

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Test 3: Environment Isolation Test

**Purpose:** Verify your virtual environment is isolated from system Python.

```python
import sys
import site

print("Python executable:", sys.executable)
print("\nSite packages:")
for path in site.getsitepackages():
    print(f"  {path}")
```

**✅ Confirmation:** All paths should reference your `.venv` directory, not system Python.

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## ⚙️ Troubleshooting

### Issue 1: Virtual Environment Not Appearing in Kernel List

**Symptoms:**
- Kernel registered successfully but not visible in Jupyter

**Solutions:**

1. **Restart Jupyter Server**
   ```bash
   # Stop current server (Ctrl+C in terminal)
   # Restart
   jupyter notebook
   ```

2. **Verify Kernel Installation**
   ```bash
   jupyter kernelspec list
   ```
   
   **Expected Output:**
   ```
   Available kernels:
     .venv      /home/user/.local/share/jupyter/kernels/.venv
     python3    /usr/share/jupyter/kernels/python3
   ```

3. **Re-register Kernel**
   ```bash
   # Remove existing kernel
   jupyter kernelspec uninstall .venv
   
   # Re-register
   python -m ipykernel install --user --name .venv --display-name "Python (.venv)"
   ```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Issue 2: ModuleNotFoundError Despite Package Installation

**Symptoms:**
- Packages installed in venv but import fails in Jupyter

**Root Causes & Solutions:**

1. **Wrong Kernel Selected**
   - **Check:** Top-right corner of notebook
   - **Fix:** Kernel → Change Kernel → Python (.venv)

2. **Package Installed Outside Virtual Environment**
   - **Check:** Verify venv activation: `(.venv)` in terminal
   - **Fix:** Activate venv, then `pip install package_name`

3. **Stale Kernel Cache**
   - **Fix:** Restart kernel: Kernel → Restart

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Issue 3: Permission Denied Errors

**Symptoms:**
```
PermissionError: [Errno 13] Permission denied
```

**Solutions:**

1. **Use `--user` Flag**
   ```bash
   python -m ipykernel install --user --name .venv --display-name "Python (.venv)"
   ```

2. **Check Directory Permissions**
   ```bash
   # macOS/Linux
   ls -la ~/.local/share/jupyter/kernels/
   
   # Fix permissions if needed
   chmod -R 755 ~/.local/share/jupyter/kernels/
   ```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Issue 4: Kernel Dies Immediately After Starting

**Symptoms:**
- Kernel connects then immediately disconnects
- "Dead kernel" error message

**Solutions:**

1. **Check Python Version Compatibility**
   ```bash
   python --version  # Should be 3.8+
   ```

2. **Reinstall ipykernel**
   ```bash
   pip uninstall ipykernel
   pip install ipykernel
   ```

3. **Check Jupyter Logs**
   ```bash
   jupyter notebook --debug
   ```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## 🌟 Best Practices

### 1. Virtual Environment Management

#### Naming Conventions

```bash
# ✅ Good: Descriptive and clear
python -m venv venv_ml_project
python -m venv .venv_data_analysis

# ❌ Avoid: Generic names for multiple projects
python -m venv venv  # Confusing if you have multiple projects
```

#### Location

```bash
# ✅ Recommended: Inside project directory
project/
├── .venv/
├── notebooks/
├── src/
└── requirements.txt

# ❌ Avoid: Shared location for multiple projects
~/virtualenvs/all_projects/  # Hard to manage
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### 2. Dependency Management

#### Create Requirements File

```bash
# After installing all packages
pip freeze > requirements.txt
```

#### Reproduce Environment

```bash
# On new machine or for team member
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
pip install -r requirements.txt
python -m ipykernel install --user --name project_name --display-name "Python (Project)"
```

#### Pin Versions for Stability

```txt
# requirements.txt
pandas==2.1.0
numpy==1.24.3
scikit-learn==1.3.0
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### 3. One Environment Per Project

**Recommended Structure:**

```
projects/
├── project_a/
│   ├── .venv/           # Project A's isolated environment
│   ├── notebooks/
│   └── requirements.txt
│
├── project_b/
│   ├── .venv/           # Project B's isolated environment
│   ├── notebooks/
│   └── requirements.txt
```

**Benefits:**
- Prevents dependency conflicts
- Easy to delete/recreate environments
- Clear project boundaries
- Simplified debugging

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### 4. Global Jupyter Installation (Optional)

**Strategy:** Install Jupyter once globally, create virtual environments for projects.

```bash
# One-time global installation
pip install jupyter

# For each project
cd project_directory
python -m venv .venv
source .venv/bin/activate
pip install ipykernel
python -m ipykernel install --user --name project_name --display-name "Python (Project Name)"
```

**Advantage:** Saves disk space and installation time.

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### 5. Version Control Integration

#### `.gitignore` Configuration

```gitignore
# Virtual environment
.venv/
venv/
env/

# Jupyter Notebook checkpoints
.ipynb_checkpoints/

# Python cache
__pycache__/
*.pyc
```

#### Track Requirements

```bash
# Always commit requirements.txt
git add requirements.txt
git commit -m "Update project dependencies"
```

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## 🆚 Comparison with Alternative Methods

### Method 1: This Guide (venv + ipykernel)

| Aspect | Rating | Notes |
|--------|--------|-------|
| **Disk Usage** | ⭐⭐⭐⭐⭐ | ~50-100MB per environment |
| **RAM Usage** | ⭐⭐⭐⭐⭐ | Minimal overhead |
| **VM Compatibility** | ⭐⭐⭐⭐⭐ | Excellent for cloud/VM |
| **Setup Complexity** | ⭐⭐⭐⭐ | Simple, one-time setup |
| **Industry Standard** | ⭐⭐⭐⭐⭐ | Standard Python practice |

---

### Method 2: Anaconda/Conda

| Aspect | Rating | Notes |
|--------|--------|-------|
| **Disk Usage** | ⭐⭐ | 3-5GB base installation |
| **RAM Usage** | ⭐⭐⭐ | Higher memory footprint |
| **VM Compatibility** | ⭐⭐ | Heavy for VMs |
| **Setup Complexity** | ⭐⭐⭐⭐⭐ | Very beginner-friendly |
| **Industry Standard** | ⭐⭐⭐⭐ | Popular in data science |

**When to Use Anaconda:**
- You need pre-compiled scientific libraries
- You're a complete beginner
- You have abundant system resources (8GB+ RAM)
- You need GUI package management

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Method 3: Docker Containers

| Aspect | Rating | Notes |
|--------|--------|-------|
| **Disk Usage** | ⭐⭐⭐ | 500MB-2GB per container |
| **RAM Usage** | ⭐⭐⭐ | Moderate overhead |
| **VM Compatibility** | ⭐⭐⭐⭐ | Excellent isolation |
| **Setup Complexity** | ⭐⭐ | Requires Docker knowledge |
| **Industry Standard** | ⭐⭐⭐⭐⭐ | Production standard |

**When to Use Docker:**
- Production deployment
- Complex multi-service setups
- Team collaboration with exact environment replication
- You need OS-level isolation

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

### Recommendation Matrix

| Use Case | Recommended Method |
|----------|-------------------|
| **Learning Python/Data Science** | Anaconda (beginner-friendly) |
| **Professional Development** | venv + ipykernel (this guide) |
| **Resource-Constrained Systems** | venv + ipykernel |
| **Production Deployment** | Docker |
| **Team Collaboration** | venv + requirements.txt or Docker |
| **ML Model Development** | venv + ipykernel or Conda |

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## 🏁 Conclusion

### Key Takeaways

1. **Jupyter does not auto-detect virtual environments** — you must register them as kernels
2. **`ipykernel` bridges virtual environments and Jupyter** through kernel registration
3. **This method is lightweight and industry-standard** — ideal for production-like workflows
4. **One environment per project** prevents dependency conflicts and simplifies management

### When to Use This Approach

✅ **Ideal For:**
- Professional Python development
- Cloud/VM deployments
- CI/CD pipelines
- Team projects with version control
- Resource-constrained environments

❌ **Not Ideal For:**
- Complete beginners (consider Anaconda)
- Complex scientific computing requiring pre-compiled binaries
- Organizations already standardized on Conda

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## 📚 Additional Resources

### Official Documentation

- [Python venv Documentation](https://docs.python.org/3/library/venv.html)
- [Jupyter Kernels Documentation](https://jupyter-client.readthedocs.io/en/stable/kernels.html)
- [IPython Kernel Installation](https://ipython.readthedocs.io/en/stable/install/kernel_install.html)

### Recommended Reading

- [Python Virtual Environments: A Primer](https://realpython.com/python-virtual-environments-a-primer/)
- [Managing Jupyter Notebooks](https://jupyter-notebook.readthedocs.io/en/stable/)
- [pip User Guide](https://pip.pypa.io/en/stable/user_guide/)

### Video Tutorial

🎥 **Watch the step-by-step video tutorial:** [Link to your video/reel]

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## 👨‍💻 About the Author

<div align="center">

<img src="https://github.com/coder-akram-khan.png" width="120" style="border-radius: 50%;" alt="Akram Khan">

### **Akram Khan**

*Data Analyst | ML Engineer*

</div>

I'm passionate about making data science accessible through clear, practical tutorials. With experience in analytics, machine learning, and cloud technologies, I create content that bridges the gap between theory and production.

**Specializations:**
- 📊 Business Analytics & BI
- 🤖 Machine Learning & AI
- ☁️ Azure Cloud Architecture
- 🐍 Python Development
- 📈 Data Visualization

**Why I Create These Tutorials:**
> "I've spent countless hours debugging environments, figuring out deployments, and learning from scattered resources. These tutorials are what I wish I had when I started - comprehensive, practical, and production-ready."

📫 **Connect with me:**
- GitHub: [@coder-akram-khan](https://github.com/coder-akram-khan)
- LinkedIn: [Akram Khan](https://linkedin.com/in/mr-akram-khan)
- Email: akram.codes.it@gmail.com

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## ⭐ Support This Guide

If this guide helped you:

- ⭐ **Star this repository** on GitHub
- 🔁 **Share** with fellow developers
- 💬 **Open an issue** for questions or improvements
- 🤝 **Contribute** with pull requests

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

## 📝 License

This guide is released under the [MIT License](LICENSE).

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

**Last Updated:** February 2024  
**Version:** 1.0.0

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/solar.png" width="100%" />

<div align="center">

**Happy Coding! 🚀**

*Building better workflows, one virtual environment at a time.*

</div>










