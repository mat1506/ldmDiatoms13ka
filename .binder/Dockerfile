FROM rocker/binder:4.3.3
LABEL maintainer='Matías Frugone-Álvarez <matutefrugone@gmail.com>'
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    libxml2-dev libcurl4-openssl-dev libssl-dev \
    libfontconfig1-dev libfreetype6-dev libharfbuzz-dev libfribidi-dev \
    libudunits2-dev libgdal-dev libgeos-dev libproj-dev \
    && rm -rf /var/lib/apt/lists/*

ENV CRAN_REPO="https://cloud.r-project.org"
ENV RENV_CONFIG_REPOS_OVERRIDE=${CRAN_REPO}
ENV RENV_PATHS_CACHE=/home/${NB_USER}/.cache/R/renv

COPY --chown=${NB_USER}:${NB_USER} renv.lock /home/${NB_USER}/renv.lock
COPY --chown=${NB_USER}:${NB_USER} .Rprofile /home/${NB_USER}/.Rprofile
COPY --chown=${NB_USER}:${NB_USER} . /home/${NB_USER}

# 5) Restaurar paquetes con renv (como usuario NB_USER)
USER ${NB_USER}
WORKDIR /home/${NB_USER}
RUN R -q -e "options(repos=c(CRAN='${CRAN_REPO}')); \
             install.packages('renv'); \
             renv::restore(prompt = FALSE)"

# Listo: Binder arrancará en /home/${NB_USER} con los paquetes restaurados

RUN wget https://github.com/<USERNAME>/<REPO>/raw/master/DESCRIPTION && R --vanilla -s -e "options(repos = list(CRAN = 'http://cran.rstudio.com'));  install.packages('renv'); renv::restore()"

RUN rm DESCRIPTION.1; exit 0
