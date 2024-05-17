# Python package making
## Fast
### Build setup.py
```bash
python setup.py sdist bdist_wheel
```
### Build project.toml
```bash
python -m build
```

```bash
python -m twine upload dist/\*
```

## Detailed

1. Project Structure:
   - Create a project directory with a descriptive name for your package.
   - Inside the project directory, create a subdirectory with the same name as your package.
   - Add a `__init__.py` file inside the package directory to make it a Python package.
   - Place your package code and other necessary files inside the package directory.

2. Setup Configuration:
   - Create a `setup.py` file in the project directory to define the package metadata.
   - Inside `setup.py`, import `setuptools` and define the `setup()` function.
   - Set the required package metadata, such as `name`, `version`, `description`, `author`, `author_email`, `url`, etc.
   - Specify the package dependencies using the `install_requires` parameter.

3. README and License:
   - Create a `README.md` file in the project directory to provide information about your package.
   - Include details about installation, usage, and any other relevant instructions.
   - Add a license file (e.g., `LICENSE.txt`) to define the license under which your package is released.

4. Packaging:
   - Open a terminal and navigate to the project directory.
   - Ensure you have `setuptools` and `wheel` packages installed (you can use `pip install setuptools wheel`).
   - Run `python setup.py sdist bdist_wheel` to generate the source distribution and wheel distribution files.

5. Uploading to PyPI:
   - Register an account on PyPI (https://pypi.org/) if you haven't already.
   - Install `twine` package if not already installed (use `pip install twine`).
   - Run `twine upload dist/*` to upload the distribution files to PyPI.
   - Enter your PyPI username and password when prompted.

6. Verification:
   - Visit your package page on PyPI to verify that your package is successfully uploaded and displayed correctly.
   - Test the installation process by creating a virtual environment and installing your package using `pip`.

7. Updating:
   - To update your package, increment the version number in `setup.py`.
   - Repeat steps 4-6 to generate and upload the updated distribution files.

That's it! Following these steps will help you build your Python package and distribute it on PyPI for others to use.
