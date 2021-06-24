# Deep Learning Tookit for Splunk

<https://www.splunk.com/en_us/blog/tips-and-tricks/splunk-with-the-power-of-deep-learning-analytics-and-gpu-acceleration.html>
<https://anthonygtellez.github.io/2020/01/10/Creating-Custom-Containers-DLTK.html>

## Overview

The Deep Learning Toolkit App for Splunk ( DLTK ) allows you to integrate advanced custom machine learning systems with the Splunk platform. It extends Splunk’s Machine Learning Toolkit ( MLTK ) with prebuilt Docker containers for TensorFlow, PyTorch and a collection of NLP and classical machine learning libraries. By using predefined workflows for rapid development with Jupyter Lab Notebooks the app enables you to build, test (e.g. using TensorBoard) and operationalise your customised models with Splunk. You can leverage GPUs for compute intense training tasks and flexibly deploy models on CPU or GPU enabled containers. The app ships with various examples that showcase different deep learning and machine learning algorithms for classification, regression, forecasting, clustering, graph analytics and NLP. This allows you to tackle advanced data science use cases in Splunk’s main areas of IT Operations, Security, Application Development, IoT, Business Analytics and beyond.

- Splunk 용 Deep Learning Toolkit 앱(DLTK)을 사용하면 고급 사용자지정 기계학습 시스템을 Splunk 플랫폼과 통합
- TensorFlow, PyTorch 및 NLP 및 클래식 머신러닝 라이브러리 모음을 위해 사전 구축된 Docker 컨테이너로 Splunk의 MLTK(Machine Learning Toolkit)를 확장
- Jupyter Lab Notebooks로 신속한 개발을 위해 사전 정의된 워크플로를 사용하면 앱을 통해 Splunk로 맞춤형 모델을 구축, 테스트(예 : TensorBoard 사용) 및 운영
- 컴퓨팅 집약적인 교육 작업에 GPU를 활용하고 CPU 또는 GPU 지원 컨테이너에 모델을 유연하게 배포
- 이 앱은 분류, 회귀, 예측, 클러스터링, 그래프 분석 및 NLP를 위한 다양한 딥러닝 및 머신러닝 알고리즘을 보여주는 다양한 예제와 함께 제공
- 이를 통해 Splunk의 IT 운영, 보안, 애플리케이션 개발, IoT, 비즈니스 분석 등의 주요 영역에서 고급 데이터 과학 사용사례를 해결

### Release Notes

Updated container images for:

- Golden Image CPU
- Golden Image GPU
- Rapids 0.17
- Spark 3.0.1

With improvements for:
- Model management with MLflow
- Integrated GIT version control in Jupyter Lab
- HTTPS for api and jupyter endpoints

Newly added algorithm examples:
- Matrix profiles with STUMPY
- Changepoint Detection
- Multivariate LSTM Regressor

## Details

### Deep Learning Toolkit for Splunk (DLTK)

The latest version of DLTK 4.x is available open source on GitHub: <https://github.com/splunk/deep-learning-toolkit> - feel free to open issues or join the community and actively contribute. For DLTK 3.x feel free to open issues, report bugs or raise feature requests on <https://github.com/splunk/splunk-mltk-container-docker>. This app is community supported, please also refer to [community.splunk.com](https://community.splunk.com/t5/tag/Deep%20Learning%20Toolkit%20for%20Splunk/tg-p/board-id/apps-add-ons-all), post your questions and engage with answers. Thanks for your collaboration!

- 최신 버전의 DLTK 4.x는 GitHub에서 오픈소스로 사용
- DLTK 3.x의 경우 <https://github.com/splunk/splunk-mltk-container-docker>에서 문제를 열거나 버그를 보고하거나 기능 요청을 제기
- community.splunk.com을 참조하고 질문을 게시하고 답변에 참여

### Prerequisites

- Splunk [Machine Learning Toolkit](https://splunkbase.splunk.com/app/2890/) installed
- [Docker](https://www.docker.com/) OR [Kubernetes](https://kubernetes.io/) OR [OpenShift](https://www.openshift.com/) environment

### Quick start guide

- Ensure Splunk Machine Learning Toolkit is installed and configured properly for your Splunk deployment.
- Restart your Splunk instance after installing the Machine Learning Toolkit and the Deep Learning Toolkit App for Splunk.
- You need to have an internet connected Docker environment accessible with permissions to pull the prebuilt MLTK container images from Dockerhub and start containers. If you are running Docker in an air-gapped environment read the description in the app.
- Setup the Deep Learning Toolkit App for Splunk by connecting it to your Docker environment using the setup page in the app.
- Start a development container from the container management dashboard and depending on your selected image run one of the examples to verify that the Deep Learning Toolkit app works:
- Neural Network Classifier Example for Tensorflow
- Logistic Regression Classifier Example for PyTorch

- Splunk Machine Learning Toolkit이 Splunk 배포에 맞게 설치 및 구성되었는지 확인
- Machine Learning Toolkit 및 Splunk 용 Deep Learning Toolkit 앱을 설치한 후 Splunk 인스턴스를 다시 시작
- Dockerhub에서 미리 빌드 된 MLTK 컨테이너 이미지를 가져와 컨테이너를 시작할 수있는 권한으로 액세스 할 수있는 인터넷 연결 Docker 환경이 있어야합니다. Air-gapped 환경에서 Docker를 실행하는 경우 앱의 설명 참조
- 앱의 설정 페이지를 사용하여 Docker 환경에 연결하여 Splunk 용 Deep Learning Toolkit 앱을 설정
- 컨테이너 관리 대시 보드에서 개발 컨테이너를 시작하고 선택한 이미지에 따라 예제 중 하나를 실행하여 Deep Learning Toolkit 앱이 작동하는지 확인
- Tensorflow를 위한 신경망 분류기 예제
- PyTorch의 로지스틱 회귀 분류기 예제

### Build your own containers

Extend the app with custom MLTK Containers: if you want to rebuild the existing MLTK Container images or want to build your own custom images navigate to https://github.com/splunk/splunk-mltk-container-docker
사용자지정 MLTK 컨테이너로 앱 확장 : 기존 MLTK 컨테이너 이미지를 다시 빌드하거나 사용자지정 이미지를 빌드하려면 <https://github.com/splunk/splunk-mltk-container-docker>로 이동

### Further information, recent blog posts and additional resources

- [Deep Learning Toolkit 3.5 - Part 2: Change Point Detection, Matrix Profiles and LSTM-based Predictions](https://www.splunk.com/en_us/blog/platform/deep-learning-toolkit-3-5-part-2-change-point-detection-matrix-profiles-and-lstm-based-predictions.html)
- [Deep Learning Toolkit 3.5 - Part 1: Git, MLflow and Image Updates](https://www.splunk.com/en_us/blog/platform/deep-learning-toolkit-3-5-part-1-git-mlflow-and-image-updates.html)
- [Walkthrough to Set Up the Deep Learning Toolkit for Splunk with Amazon EKS](https://www.splunk.com/en_us/blog/platform/walkthrough-to-set-up-the-deep-learning-toolkit-for-splunk-with-amazon-eks.html) - thanks Junichi!
- [Deep Learning Toolkit 3.4: Grid Search, Causal Inference and Process Mining](https://www.splunk.com/en_us/blog/platform/deep-learning-toolkit-3-4-grid-search-causal-inference-and-process-mining.html)
- [Causal Inference: Determining Influence in Messy Data](https://www.splunk.com/en_us/blog/platform/causal-inference-determining-influence-in-messy-data.html) - thanks Greg!
- .conf20 [presentation](https://conf.splunk.com/watch/conf-online.html?search=pla1300c#/) and [recording](https://conf.splunk.com/watch/conf-online.html?search=pla1300c#/) Advances in Deep Learning Toolkit 4.0: Deploy, Observe and Scale your Machine Learning Projects for Splunk with Spark, TensorFlow, PyTorch, Rapids and Dask
- [Detailed walkthrough how to setup DLTK on a AWS GPU instance](https://www.splunk.com/en_us/blog/tips-and-tricks/splunk-with-the-power-of-deep-learning-analytics-and-gpu-acceleration.html) - thanks Dimitris!
- [Deep Learning Toolkit 3.3 - Examples for Explainable AI and XGBoost](https://www.splunk.com/en_us/blog/machine-learning/deep-learning-toolkit-3-3-examples-for-explainable-ai-and-xgboost.html)
- [Deep Learning Toolkit 3.2 - Graphics, Rapids, Spark and More](https://www.splunk.com/en_us/blog/machine-learning/deep-learning-toolkit-3-2-graphics-rapids-spark-and-more.html)
- [DLTK 3.1 release for Kubernetes and Openshift](https://www.splunk.com/en_us/blog/machine-learning/deep-learning-toolkit-3-1-release-for-kubernetes-and-openshift.html)
- [DLTK 3.1 new examples for Prophet, Graphs, DASK and GPU](https://www.splunk.com/en_us/blog/machine-learning/deep-learning-toolkit-3-1-examples-for-prophet-graphs-gpus-and-dask.html)
- .conf19 [presentation](https://conf.splunk.com/files/2019/slides/FN1409.pdf) and [recording](https://conf.splunk.com/files/2019/recordings/FN1409.mp4)
- [Detailed Walkthrough for NLP: Named Entity Recognition and Extraction](https://www.splunk.com/en_us/blog/tips-and-tricks/named-entity-recognition-and-extraction.html) - thanks Greg!
- [Introduction to DLTK 3.0 release on splunk blogs](https://www.splunk.com/en_us/blog/machine-learning/deep-learning-toolkit-3-0-release.html)
- [Detailed walkthrough to create custom container images for DLTK](https://www.splunk.com/en_us/blog/machine-learning/using-docker-and-splunk-to-operationalize-the-machine-learning-toolkit1.html) - thanks Anthony! mirror link
- [Detailed walkthrough in Japanese by @maroon](https://qiita.com/maroon/items/5a8b027631a674d6d8be)
- [TensorFlow World 2019 Poster Session with PDF download in high resolution](https://conferences.oreilly.com/tensorflow/tf-ca-2019/public/schedule/detail/80667)
- .conf18 [presentation including CPU/GPU benchmarks](https://static.rainfocus.com/splunk/splunkconf18/sess/1523455772333001KEn6/finalPDF/Dockerized-Deep-Learning-1478_1538787890679001YxJP.pdf)

### List of available algorithm examples (31)

- Binary neural network classifier build on keras and TensorFlow
- Logistic regression using PyTorch
- Multi class neural network classifier using PyTorch with GPU
- Multi class neural network classifier using PyTorch for DGA
- Gradient boosting model with Spark's MLLib applied to the DGA dataset
- XGBoost classifier with SHAP explainability
- Linear regression using the TensorFlow™ estimator class
- Regression using the TensorFlow™ Deep Neural Network (DNN) estimator class
- XGBoost regressor
- Support vector regressor with grid search
- Multivariate LSTM regressor
- Forecasting time series using TensorFlow (CNN)
- Forecasting time series using TensorFlow (LSTM)
- Forecasting time series using the Prophet library
- Basic auto encoder using TensorFlow™
- Distributed algorithm execution with DASK for KMeans
- Clustering with UMAP and DBSCAN
- Named Entity Recognition using spaCy for NLP tasks
- Named Entity Recognition using spaCy Ginza (Japanese)
- Graph centrality algorithms using NetworkX
- Graph community detection with Rapids (GPU accelerated)
- Causal Inference with causalnex
- Frequent itemset mining with Spark FP Growth
- Recommender system with Spark Collaborative Filtering
- Rapids UMAP (GPU accelerated)
- Process Mining with PM4Py
- Time series analysis with STUMPY
- Changepoint Detection
- DGA Datashader Visualization Example
- Correlation Matrix and Pair Plot
- Spark Pi Hello World example

### FAQ

Q: When I launch a container the first time, I cannot access Jupyter Lab.
A: The selected container image will be downloaded from dockerhub automatically in the background when you launch a container for the first time. Depending on your network this can take a while to download the docker image for the first time as the image sizes range from 2-12 GB. Please allow for some time to get the images initially pulled from Dockerhub. You can check which docker images are available locally by running docker images on your CLI.

Q : 컨테이너를 처음 시작할 때 Jupyter Lab에 액세스 할 수 없습니다.
A : 컨테이너를 처음 시작하면 선택한 컨테이너 이미지가 백그라운드에서 자동으로 dockerhub에서 다운로드됩니다. 네트워크에 따라 이미지 크기가 2 ~ 12GB 범위이므로 처음으로 도커 이미지를 다운로드하는 데 시간이 걸릴 수 있습니다. Dockerhub에서 처음에 이미지를 가져올 때까지 잠시 기다리십시오. CLI에서 Docker 이미지를 실행하여 로컬에서 사용할 수있는 Docker 이미지를 확인할 수 있습니다.

Q: When I run DLTK 3.5 locally my browser showes insecure connection
A: From DLTK 3.5 onwards the container images use HTTPS by default with self signed certificates for the data transfer related api endpoints and Jupyter Lab. Many browsers show "insecure connection" warnings and some allow to suppress that for localhost connections used during development. For production use, please work with your admins to secure your setup and build containers with your own certificates as needed.

Q : DLTK 3.5를 로컬에서 실행하면 브라우저에 안전하지 않은 연결이 표시됩니다.
A : DLTK 3.5부터 컨테이너 이미지는 기본적으로 데이터 전송 관련 API 엔드 포인트 및 Jupyter Lab에 자체 서명 된 인증서와 함께 HTTPS를 사용합니다. 많은 브라우저가 "안전하지 않은 연결"경고를 표시하고 일부는 개발 중에 사용되는 로컬 호스트 연결에 대해이를 억제 할 수 있습니다. 프로덕션 용도의 경우 관리자와 협력하여 설정을 보호하고 필요에 따라 자체 인증서로 컨테이너를 빌드하세요.

Q: The example dashboards show no results or throw errors.
A: First, ensure that the right container image is downloaded and up and running for the specific example (e.g. TensorFlow examples require a TensorFlow container). Secondly, ensure that you have verified the associated notebook code exists in Juypter Lab and you have explicitly saved the notebook again (hit save button). By doing this, a python module is saved automatically generated (located in the /app/model folder in Juypter) which is needed to run the examples and populate the dashboards. Lastly, please check if MLTK's app permissions are set to global so that DLTK can use the lookup files used in most of the examples.

Q : 예제 대시 보드에는 결과가 표시되지 않거나 오류가 발생합니다.
A : 먼저 특정 예제에 적합한 컨테이너 이미지가 다운로드되고 실행되고 있는지 확인합니다 (예 : TensorFlow 예제에는 TensorFlow 컨테이너가 필요함). 둘째, Juypter Lab에 관련 노트북 코드가 있는지 확인하고 노트북을 명시 적으로 다시 저장했는지 확인합니다 (저장 버튼 누르기). 이렇게하면 예제를 실행하고 대시 보드를 채우는 데 필요한 Python 모듈이 자동으로 생성되어 저장됩니다 (Juypter의 /app/model 폴더에 있음). 마지막으로, DLTK가 대부분의 예제에서 사용되는 조회 파일을 사용할 수 있도록 MLTK의 앱 권한이 전역으로 설정되어 있는지 확인하십시오.

Q: Where are my notebooks stored in the docker environment?
A: By default, there are 2 docker volumes automatically mounted for persistance in your docker environment. Those volumes are named "mltk-container-app" and "mltk-container-notebooks". You can verify by running docker volume ls on your CLI. Important note: from DLTK version 3.1 onwards there is a new default volume called "mltk-container-data" - see migration notes below.

Q : 내 노트북은 Docker 환경에서 어디에 저장?
A : 기본적으로 Docker 환경에서 지속성을 위해 자동으로 마운트된 2개의 Docker 볼륨이 있습니다. 이러한 볼륨의 이름은 "mltk-container-app"및 "mltk-container-notebooks"입니다. CLI에서 docker volume ls를 실행하여 확인할 수 있습니다.
중요 참고 사항 : DLTK 버전 3.1부터는 "mltk-container-data"라는 새로운 기본 볼륨이 있습니다. 아래 마이그레이션 참고 사항을 참조

Q: What is the password for Jupyter Lab?
A: Please have a look at the Model Development Guide page in the Deep Learning Toolkit app.

Q : Jupyter Lab의 비밀번호는 무엇?
A : Deep Learning Toolkit 앱의 Model Development Guide 페이지를 참조

### Notebooks Migration note for change to 3.1 and later versions

Due to the addition of Kubernetes there was a change and addition made to the way how volumes behave. From 3.1 on and with the use of the golden image the container directory /srv is the default and notebooks and app code is in there. In earlier versions there were two docker volumes mounted into /srv/app and /srv/notebooks which will be mapped into a backup folder in Jupyter from version 3.1 on. For migration simply copy your notebooks from the backup folder back into the notebooks and app folder in case those are empty.

- Kubernetes의 추가로 인해 볼륨이 작동하는 방식이 변경되고 추가
- 3.1부터 골든 이미지를 사용하면 컨테이너 디렉토리 /srv가 기본값이고 노트북 및 앱 코드가 있음
- 이전 버전에는 /srv/app 및 /srv/notebooks에 마운트된 두 개의 도커 볼륨이 있었으며 버전 3.1부터 Jupyter의 백업 폴더에 매핑
- 마이그레이션을 위해 노트북이 비어 있는 경우 백업 폴더에서 노트북 및 앱 폴더로 노트북을 복사