FROM image-registry.openshift-image-registry.svc:5000/redhat-ods-applications/pytorch:2025.2

COPY requirements.txt /opt/app-root/bin/

RUN pip install -r /opt/app-root/bin/requirements.txt --no-build-isolation && rm /opt/app-root/bin/requirements.txt