# gcp-fastapi


### Install pyenv for creating virtual env

- Install pyenv follows the [ref](https://github.com/pyenv/pyenv-installer).

    ```bash
    curl https://pyenv.run | bash
    ```

- Add these lines to `.bashrc` file

    ```bash
    export PATH="/home/kimdoanh89/.pyenv/bin:$PATH"
    eval "$(pyenv init -)"
    eval "$(pyenv virtualenv-init -)"
    ```
- Restart shell then update pyenv

    ```bash
    exec $SHELL
    pyenv update
    ```

- Check python version with `pyenv install -l | grep 3.9`
- Install with `pyenv install 3.9.0`
- Set global python version to 33.9.0 with `pyenv global 3.9.0`.

### Install poetry for python packages management

- Follows the instructions [here](https://python-poetry.org/docs/).

    ```bash
    curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
    ```

- Modify `.bashrc` file

    ```bash
    export PATH="/home/kimdoanh89/.pyenv/bin:$HOME/.poetry/bin:$PATH"
    eval "$(pyenv init -)"
    eval "$(pyenv virtualenv-init -)"
    ```

- Config poetry with

    ```bash
    poetry config virtualenvs.in-project true
    poetry config --list
    ```

### Set up project with poetry

- Init poetry
    ```bash
    poetry init
    ```

- Add required packages `fastapi` and `requests`. Reference is [here](https://fastapi.tiangolo.com/).

    ```bash
    poetry add fastapi
    poetry add requests
    ```

- We will also need an ASGI server, for production such as `Uvicorn` or `Hypercorn`.

    ```bash
    poetry add uvicorn
    ```

- Generate requirements file with poetry

    ```bash
    poetry export -f requirements.txt > requirements.txt
    ```

- Run the server with

    ```bash
    uvicorn main:app --reload
    ```

- Interactive docs:
  - Go to http://127.0.0.1:8000/docs
  - Or go to http://127.0.0.1:8000/redoc

### Deploy on GCP

- Go to [GCP console](https://console.cloud.google.com/) and create a new project `gcp-fastapi`

- Set the project to host the Google App Engine

    ```bash
    gcloud config set project gcp-fastapi
    ```
- Create `app.yaml` file

    ```yaml
    runtime: python39
    entrypoint: uvicorn main:app --reload
    ```

- Create app and deploy it

    ```bash
    gcloud app create
    gcloud app deploy
    ```

- See the link with `gcloud app browse`.

- The app is deployed [here](https://gcp-fastapi.ey.r.appspot.com).
- The REST API documentation is as follows:
  - [Swagger UI style](https://gcp-fastapi.ey.r.appspot.com/docs)
  - [Redoc style](https://gcp-fastapi.ey.r.appspot.com/redoc)

## Automating App Engine Deployment
[Reference](https://cloud.google.com/source-repositories/docs/quickstart-triggering-builds-with-source-repositories).

- Create `cloudbuild.yaml` file

    ```yaml
    steps:
    - name: "gcr.io/cloud-builders/gcloud"
    args: ["app", "deploy"]
    timeout: "1600s"
    ```


Follow the steps from ref.
- Enable the APIs.
- Grant App Engine access to the Cloud Build service account.
  - Enable App Engine
- Enable the Build Trigger
