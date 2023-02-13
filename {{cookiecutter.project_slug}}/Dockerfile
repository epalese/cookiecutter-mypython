FROM {{cookiecutter.docker_image_tag}}

WORKDIR /usr/src/app

COPY {{cookiecutter.project_slug}} ./{{cookiecutter.project_slug}}
COPY tests ./tests
COPY requirements_dev.txt ./

ENV PYTHONPATH=/usr/src/app/{{cookiecutter.project_slug}}

RUN pip install --no-cache-dir -r {{cookiecutter.requirements_file}}

CMD ["/bin/sh", "-c", "black --check /usr/src/app/{{cookiecutter.project_slug}} /usr/src/app/tests && pytest --cov=/usr/src/app/{{cookiecutter.project_slug}}"]
