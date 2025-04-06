Things I learned or had to alter to get this working: 
1) Need to ensure that virtualenv is created and active (My resolution: `poetry shell`, after altering the config to include python venv config with `    virtualenv: venv`)

2) Need to ensure all proper dependencies have been installed (`poetry add setuptools`)

3) Ensure that gcloud auth has been setup with application adc login (in a production context, you'd setup OIDC for this)

4) Add resource for all relevant APIs to be activated within GCP (In a production context, this would be writing new resources that activate the APIs, not doing it manually with gcloud commands)

... but in the end, that was fun :) 