clean: clean-pyc clean-test

clean-pyc: ## remove Python file artifacts
	find . -name '*.pyc' -exec rm -f {} +
	find . -name '*.pyo' -exec rm -f {} +
	find . -name '*~' -exec rm -f {} +
	find . -name '__pycache__' -exec rm -fr {} +	

clean-test:
	rm -fr .pytest_cache

build-requirements:
	pip install --no-cache-dir pip-tools
	pip-compile -o requirements.txt pyproject.toml
	pip-compile --extra dev -o requirements_dev.txt pyproject.toml

lint:
	black "{{ cookiecutter.project_slug }}" "tests"

test: build-test run-test

build-test:
	docker build -t "{{ cookiecutter.project_slug }}:latest" .

run-test:
	docker run --rm -it "{{ cookiecutter.project_slug }}:latest"

run-test-mount:
	docker run --rm --mount type=bind,source="`pwd`/{{ cookiecutter.project_slug }},target=/usr/src/app/my_first_cookie --mount type=bind,source="`pwd`/tests/,target="/usr/src/app/tests" -it "{{ cookiecutter.project_slug }}:latest"
