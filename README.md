## Yay! I did a hackathon for Dev.to this weekend with Pulumi and it was fun!

## Prerequisites for using this repo with links to the official docs:
* You have access to the relevant GCP project and [gcloud CLI](https://cloud.google.com/sdk/docs/install) installed.
* You have Pulumi setup and logged in.

```
brew install pulumi/tap/pulumi
pulumi login
```

* You have a Python environment setup with pyenv, a shim for Python 3.12.0 and Poetry. To install pyenv, follow these [instructions](https://github.com/pyenv/pyenv?tab=readme-ov-file#homebrew-in-macos), and for Poetry, these [instructions](https://python-poetry.org/docs/#installing-with-the-official-installer). Once done, then:

```
# This will install python 3.12.0
pyenv install 3.12.0

# Then inside of the project directory, run the below command to ensure that you're using that version for this project
pyenv local 3.12.0

# Setup a few libs for Python
poetry add setuptools
poetry add virtualenv
```

* You have a Ruby environment setup for Jekyll. To do this:
```
brew install chruby ruby-install
gem install jekyll bundler
```

## How to develop with the Jekyll site:

```
cd rsi-dev
jekyll serve
# Then navigate in your browser to preview: 
127.0.0.1:4000
```

## How to get the deployment working for yourself locally:
So long as the above configuration and tooling is already ready, you should be able to redeploy the site via Pulumi by:

```
gcloud auth application-default login
poetry shell
pulumi up
```


## Things I learned or had to alter to get the Pulumi deployment working apart from the documentation from Pulumi's site:
1) Need to ensure that virtualenv is created and active (My resolution: `poetry shell`, after altering the config to include python venv config with `    virtualenv: venv`)

2) Need to ensure all proper dependencies have been installed (`poetry add setuptools` with the new virtualenv activated)

3) Ensure that gcloud auth has been setup with application ADC login (in a production context, you'd setup OIDC for Pulumi on a Service Account for this).

4) Add resource for all relevant APIs to be activated within GCP (In a production context, this would be writing new resources that activate the APIs, not doing it manually with gcloud commands)

... but in the end, that was fun and otherwise followed the [official docs](https://www.pulumi.com/templates/static-website/gcp/) very closely.


## Things I learned or had to do to get the actual Jekyll tempalate working:
1) I found that my Ruby installation was the default, and thus, I needed to setup the latest Ruby and bundler before I could install Jekyll.

2) Once I got Jekyll rolling, it was a matter of `jekyll new` and then `jekyll serve` to make sure that my new site was at least functional.

3) I decided I wanted to use another theme instead of the default, so I went down a fun rabbit trail to figure out a new hacker Jekyll theme... but the Ruby dependencies between Jekyll and github-pages gems to preview were not working together nicely. So, I went back to the simpler clean-blog theme as suggested by their official docs.

## Downsides and admittedly problems with the current deployment:
* For all my desire to get something more pretty up, the way that the static site deployment works, it is _not_ loading any of the scss or js files. Thus, things are pretty ugly. Nevertheless, you can see the actual site here: https://storage.googleapis.com/bucket-e3362f3/index.html

Were I to go back and actually make this a personal blog site for myself, I would:
* Host this on a CloudRun instance
* Create a vanity url DNS address to forward to this backend
* Write more content and add photos.

Nevertheless, I had fun playing with these technologies and remembered how much just building stuff is fun. Thanks for the opportunity.