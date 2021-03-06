# React + Django

This is a code example for having React frontend and Django backend

## Create virtuanenv

Setup a python virtual environment within the `djangobackend` folder

`python -m venv venv`

## Start virtualenv

`venv\Scripts\activate.bat`

## Hosting React on Django backend

### 1. Build frontend

- Within the `react-frontend` folder, run `yarn build`
- This will generate a production ready set of build files as required by our Django backend

### 2. Move build folder to backend

- For the build folder generated in step (1), move it into the `djangobackend` folder, on the same level as where `manage.py` is located
- There's probably a better way to do this automatically

### 3. Prepare static files in Django

- Within the `djangobackend` folder, run `python manage.py collectstatic`
- If you have leftover static files from a previous iteration, you will be prompted to remove them

### 4. Verify everything is working

- Within the `djangobackend` folder, run `python manage.py runserver`
- Visit the development server `http://127.0.0.1:8000/`
- Try using the frontend app, every path requested should resolve with `200 OK`
- This can be observed from either Django's command line logs or using the network tab of your browser

## Dev notes

- A barebones setup for hosting React on Django has been used in `settings.py`
- This also requires the [whitenoise library](http://whitenoise.evans.io/en/stable/django.html) in order to resolve the other contents of the `build` directory such as `manifest.json`
- Do note that even without `whitenoise` it would still be able to resolve the other static files normally

### Additions to `settings.py`

1. `import os`
2. `TEMPLATES = [{..., 'DIRS': [os.path.join(BASE_DIR, 'build')], ...}]`
3. `STATICFILES_DIRS = [os.path.join(BASE_DIR, 'build', 'static')]`
4. `STATIC_ROOT = os.path.join(BASE_DIR, 'static')`
5. `MIDDLEWARE = [..., 'whitenoise.middleware.WhiteNoiseMiddleware', ...]` (Used for whitenoise, Needs to be directly after `SecurityMiddleware`)
6. `WHITENOISE_ROOT = os.path.join(BASE_DIR, 'build')` (Used for whitenoise)
