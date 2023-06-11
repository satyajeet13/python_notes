# Create Virtual Environment in Visual Studio Code

In complex software development projects, like building a Python library, an API, or software development kit, often you will be working with multiple files, multiple packages, and dependencies. As a result, you will need to isolate your Python development environment for that particular project. 

In VS Code, open Terminal (CTRL+`). The default terminal shell is PowerShell. These instructions were executed in PowerShell.

In the prompt, type the following commands to create a virtual environment:

```
PS > python -m venv virtualenv
PS > ./virtualenv/Scripts/Activate.ps1
(virtualenv) PS > python -m pip install --upgrade  pip
(virtualenv) PS > cp "D:\Scripting\Installation\requirements.txt"
(virtualenv) PS > pip install -r requirements.txt
(virtualenv) PS > cd . > sample.ipynb
(virtualenv) PS > .\sample.ipynb
```

Make sure to change the kernel to virtual enviroment kernel because that's where the packages are installed. 