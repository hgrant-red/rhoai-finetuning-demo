# Use the unauthenticated public endpoint for Red Hat UBI 9
FROM registry.access.redhat.com/ubi9/python-312:latest

# Switch to root to install system-wide packages
USER root

# Upgrade pip
RUN pip install --no-cache-dir --upgrade pip

# Install standard dependencies (Moved from Notebook Cells 1, 2, and 4)
RUN pip install --no-cache-dir \
    "datasets<4.0.0" \
    pandas \
    kubernetes==35.0.0 \
    pydantic==2.12.5 \
    transformers \
    accelerate \
    ipykernel

# Install Red Hat specialized AI packages from the internal index (Moved from Notebook Cells 3 and 4)
RUN pip install --no-cache-dir \
    kubeflow-trainer-api==2.1.0 \
    kubeflow-katib-api==0.19.0 \
    kubeflow==0.3.0+rhaiv.2 \
    training-hub==0.5.0 \
    --index-url https://console.redhat.com/api/pypi/public-rhai/rhoai/3.4/cuda12.9-ubi9/simple/ \
    --extra-index-url https://pypi.org/simple

# Revert to standard non-root user for OpenShift security compliance
USER 1001