## Creating and Activating Virtual Environments with `venv`

### 0. Installing `venv`

Before you can create virtual environments with `venv`, make sure you have Python installed on your system. Python 3.3 or later comes with `venv` built-in, so you don't need to install anything extra.

If you don't have Python installed, you can download it from the official Python website: [https://www.python.org/downloads](https://www.python.org/downloads)


### 1. Creating a Virtual Environment

To create a new virtual environment using `venv`, follow these steps:

1. Open a terminal or command prompt.
2. Navigate to the desired directory where you want to create the virtual environment.
3. Run the following command to create the virtual environment (replace `myenv` with your preferred name):

```bash
python3 -m venv myenv
```
### 2. Activating a Virtual Environment
After creating a virtual environment, you need to activate it to use it. Follow these steps:

#### On macOS and Linux:

Open a terminal.
Navigate to the directory containing the virtual environment.
Run the following command to activate the virtual environment:

```bash
source myenv/bin/activate
```
Once activated, you should see the name of the virtual environment appear in your terminal or command prompt. You can now install packages and run Python scripts within the virtual environment.

### 3. Deactivating a Virtual Environment
To deactivate the current virtual environment and return to your system's default Python environment, simply run the following command:
```bash
deactivate
```

Remember to reactivate the virtual environment whenever you want to work within it.
