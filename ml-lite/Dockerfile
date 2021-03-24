FROM python:3.8.8-slim AS dep-image

# prerequisites
RUN pip install -U pip
RUN apt-get update && \
    apt-get install -y --no-install-recommends build-essential && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install dependencies
COPY requirements.txt /tmp/requirements.txt
RUN pip install --user --quiet --no-warn-script-location -r /tmp/requirements.txt

FROM python:3.8.8-slim AS app-image
COPY --from=dep-image /root/.local /root/.local

# Make sure scripts in .local are usable:
ENV PATH=/root/.local/bin:$PATH

# project directory inside user home
WORKDIR /home

# --generate-config ensure jupyter config file is created before first launch
RUN jupyter notebook --generate-config
CMD ["jupyter", "notebook", "--port=4242", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--token=''", "--password=''"] 
