
# Use the official RHOAI 3.4 PyTorch/CUDA/Python 3.12 workbench as the base
FROM registry.redhat.io/rhoai/odh-workbench-jupyter-pytorch-cuda-py312-rhel9:v3.4

# Switch to root to install system-wide packages
USER root

# Upgrade pip
RUN pip install --no-cache-dir --upgrade pip

# Install the Hugging Face stack (These are NOT in the base image!)
RUN pip install --no-cache-dir \
    "datasets<4.0.0" \
    transformers \
    accelerate

# Install the Kubeflow and OSFT libraries from the Red Hat internal index
RUN pip install --no-cache-dir \
    kubeflow-trainer-api==2.1.0 \
    kubeflow-katib-api==0.19.0 \
    kubeflow==0.3.0+rhaiv.2 \
    training-hub==0.5.0 \
    --index-url https://console.redhat.com/api/pypi/public-rhai/rhoai/3.4/cuda12.9-ubi9/simple/ \
    --extra-index-url https://pypi.org/simple

# Revert to standard non-root user for OpenShift security compliance
USER 1001