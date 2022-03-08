FROM public.ecr.aws/sam/build-python3.9:latest AS build

COPY src/ /var/task/src
COPY LICENSE /var/task/LICENSE
COPY setup.cfg /var/task/setup.cfg
COPY pyproject.toml /var/task/pyproject.toml
COPY setup.cfg /var/task/setup.cfg


WORKDIR /var/task

RUN pip install build
RUN python -m build 



FROM public.ecr.aws/lambda/python:3.9 AS app
COPY --from=build /var/task/dist/base_library_pbardon-0.0.1.tar.gz /var/task/dist/base_library_pbardon-0.0.1.tar.gz
RUN pip install /var/task/dist/base_library_pbardon-0.0.1.tar.gz
COPY app.py ${LAMBDA_TASK_ROOT}
COPY requirements.txt ${LAMBDA_TASK_ROOT}
RUN pip install -r ${LAMBDA_TASK_ROOT}/requirements.txt

CMD ["app.lambda_handler"]