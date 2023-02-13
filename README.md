# cookiecutter-mypython  
This repository provides a [cookiecutter](https://cookiecutter.readthedocs.io/en/stable/README.html) template to set up my personal Python projects with:  
- a simple pyproject.toml with dependencies for development (`pytest` and `pip-tools`)  
- Dockerfile to run pytest tests  
- VSCode .env file with PYTHONPATH and other env variables  

## Quick start

Step 1: create a virtual environment and install cookiecutter
```
$ pyenv virtualenv 3.10.6 my_first_cookie
$ pyenv activate my_first_cookie
(my_first_cookie)$ pip install --no-cache-dir cookiecutter
```  
Note: I use pyenv to manage virtual environments but any other approach of managing virtual environments is fine.  
  
Step 2: run cookiecuter and deploy the project structure
```
(my_first_cookie)$ cookiecutter git@github.com:epalese/cookiecutter-mypython.git
full_name [Emanuele]: 
email [epalese@example.com]: 
github_username [epalese]: 
project_name [My Personal Python Boilerplate]: my_first_cookie
project_slug [my_first_cookie]: 
project_short_description [All the boilerplate I need to create my personal Python project ready for VS Code and unit testing in Docker.]: 
version [0.0.1]: 
python_version [3.10.10]: 
docker_image_tag [3.10.10-slim]: 
requirements_file [requirements_dev.txt]: 
```  
  
All directories and files should be present in `my_fitst_cookie`:
```
(my_first_cookie)$ cd my_first_cookie
(my_first_cookie)$ ls
Dockerfile      Makefile        my_first_cookie pyproject.toml  tests
```  

The final steps are to generate requirements files.  
These steps can be easily performed via a Makefile command:
```
(my_first_cookie)$ make build-requirements
```

Now the content of the `my_first_cookie` directory should be:  
```
(my_first_cookie)$ ls
Dockerfile               my_first_cookie          pyproject.toml           requirements_dev.txt
Makefile                 my_first_cookie.egg-info requirements.txt         tests
```  

You can continue installing the libraries in the local virtual environment if you wish:  
```
(my_first_cookie)$ pip install --no-cache-dir -r requirements-dev.txt
```  

The Makefile provides other useful commands:  
- `make lint`: run black to enforce proper linting (black --check is executed as part of the tests in the Dockerfile)  
- `make test`: build the Docker image and run it (this will execute the pytest tests)  
- `make run-test-mount`: run the Docker image (without building it) but mounting the local directories in the container  

## How requirements files are created
Once the directories and config files are in place we can complete the set up of our project by specifying all the dependencies in
`pyproject.toml` and generate the requirements files.  
```
(my_first_cookie)$ pip install --no-cache-dir pip-tools
(my_first_cookie)$ pip-compile --resolver=backtracking --extra dev -o requirements_dev.txt pyproject.toml
(my_first_cookie)$ pip-compile --resolver=backtracking -o requirements.txt pyproject.toml
```  

## How to test with Docker
The Dockerfile provided with the template can be used to automate testing (with pytest).  
Tests should be places in `tests/`.  

When ready you can build the image and run the container to perform the tests:  
```
(my_first_cookie)$ docker build -t "my_first_cookie:latest" .
(my_first_cookie)$ docker run --rm -it "my_first_cookie:latest"
```  

Tests can be also executed without re-building the image each time, by mounting local directories in the container:
```
(my_first_cookie)$ docker run --rm --mount type=bind,source="`pwd`/my_first_cookie,target=/usr/src/app/my_first_cookie --mount type=bind,source="`pwd`/tests/,target="/usr/src/app/tests" -it "my_first_cookie:latest"
```
