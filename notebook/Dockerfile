ARG BASE_CONTAINER=jupyter/base-notebook
FROM $BASE_CONTAINER

LABEL maintainer="rookiepeng <zpeng.me@gmail.com>"

USER $NB_UID

COPY lab /home/$NB_USER/.jupyter/lab

RUN conda install --quiet --yes \
    'numpy' \
    'matplotlib' \
    'pandas' \
    'scipy' \
    'seaborn' \
    'xlrd' \
    'psutil' \
    'autopep8' \
    'pandoc' \
    'ipywidgets' \
    'h5py'

RUN conda install --quiet --yes \
    'conda-forge::jupyter_contrib_nbextensions' \
    'conda-forge::xeus-python' \
    'conda-forge::moviepy'

RUN conda install --quiet --yes \
    'conda-forge::plotly' \
    'conda-forge::python-kaleido'

RUN conda clean --all -f -y

RUN jupyter labextension install @ryantam626/jupyterlab_code_formatter && \
    pip install jupyterlab_code_formatter && \
    jupyter serverextension enable --py jupyterlab_code_formatter && \
    jupyter lab build --dev-build=False && \
    npm cache clean --force && \
    rm -rf "$CONDA_DIR/share/jupyter/lab/staging" && \
    rm -rf "/home/$NB_USER/.cache/yarn" && \
    rm -rf "/home/$NB_USER/.node-gyp" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME /home/$NB_USER/.cache/
ENV JUPYTER_ENABLE_LAB 1

USER root
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions /home/$NB_USER

# Switch back to notebook to avoid accidental container runs as root
USER $NB_UID
