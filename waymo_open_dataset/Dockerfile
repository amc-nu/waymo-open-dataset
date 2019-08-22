FROM tensorflow/tensorflow:latest-py3

RUN apt-get update && apt-get install -y \
  git build-essential wget vim \
  pkg-config zip g++ zlib1g-dev unzip python3 python3-pip

RUN pip3 install jupyter matplotlib jupyter_http_over_ws &&\
  jupyter serverextension enable --py jupyter_http_over_ws

RUN git clone https://github.com/waymo-research/waymo-open-dataset.git waymo-od
WORKDIR /waymo-od

RUN wget https://github.com/bazelbuild/bazel/releases/download/0.28.0/bazel-0.28.0-installer-linux-x86_64.sh
RUN bash ./bazel-0.28.0-installer-linux-x86_64.sh
RUN bash ./configure.sh && bazel build waymo_open_dataset/metrics:all
RUN bash bazel build waymo_open_dataset/metrics/tools/compute_detection_metrics_main
RUN bash bazel test ...

RUN cp bazel-genfiles/waymo_open_dataset/label_pb2.py waymo_open_dataset/label_pb2.py
RUN cp bazel-genfiles/waymo_open_dataset/dataset_pb2.py waymo_open_dataset/dataset_pb2.py

EXPOSE 8888
RUN python3 -m ipykernel.kernelspec

CMD ["bash", "-c", "source /etc/bash.bashrc && PYTHONPATH=/waymo-od jupyter notebook --notebook-dir=/waymo-od --ip 0.0.0.0 --port 8888 --no-browser --allow-root"]
