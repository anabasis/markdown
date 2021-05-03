# Deep Learning Toolkit for Splunk (2.x and 3.x)

<https://github.com/splunk/splunk-mltk-container-docker>

For the latest development please check out DLTK version 4.x available on GitHub.
최신 개발에 대해서는 GitHub에서 제공되는 DLTK 버전 4.x를 확인

## MLTK Container

This repository contains the container endpoint (./app), jupyter notebook configuration (./config) and examples (./notebooks), build scripts and the main Dockerfile to create the existing pre-built container images for TensorFlow 2.0 CPU and GPU, PyTorch CPU and GPU, NLP libraries.
- 이 저장소에는 컨테이너 엔드 포인트 (./app), jupyter 노트북 구성 (./config) 및 예제(./notebooks), 빌드 스크립트 및 TensorFlow 2.0 CPU 및 GPU용으로 사전 빌드된 기존 컨테이너 이미지를 만들기 위한 기본 Dockerfile이 포함
- PyTorch CPU 및 GPU, NLP 라이브러리.

### Rebuild

You can rebuild your own containers with the build.sh script. Examples:

- Build TensorFlow CPU image for your own docker repo `./build.sh tf-cpu your_local_docker_repo/`
- Build TensorFlow GPU image for your own docker repo `./build.sh tf-gpu your_local_docker_repo/`
- Build PyTorch image for your own docker repo `./build.sh pytorch your_local_docker_repo/`
- Build NLP image for your own docker repo `./build.sh nlp your_local_docker_repo/`

If you decide to modify to `your_local_docker_repo/` you need to update your `images.conf` in the Deep Learning Toolkit app: go to your `$SPLUNK_HOME/etc/apps/mltk-container/local/images.conf` and add your own image stanzas. Have a look at `$SPLUNK_HOME/etc/apps/mltk-container/default/images.conf` to see how the stanzas are defined.
- `your_local_docker_repo/`로 수정하기로 결정한 경우 Deep Learning Toolkit 앱에서`images.conf`를 업데이트해야 합니다.`$SPLUNK_HOME/etc/apps/mltk-container/local/images.conf`로 이동하여 추가
- 자신의 이미지 스탠자. 스탠자가 정의된 방법을 보려면`$SPLUNK_HOME/etc/apps/mltk-container/default/images.conf`를 살펴보십시오.

### Build your own custom container images

Feel free to extend the build script and Dockerfile to create your own custom MLTK Container images. To make your own images available in the Deep Learning Toolkit app, please add a local config file to the app: go to your `$SPLUNK_HOME/etc/apps/mltk-container/local/images.conf` and add for example your new stanza:
- 빌드 스크립트와 Dockerfile을 자유롭게 확장하여 사용자 지정 MLTK 컨테이너 이미지를 생성
- Deep Learning Toolkit 앱에서 자신의 이미지를 사용할 수 있도록하려면 로컬 구성 파일을 앱에 추가
- `$SPLUNK_HOME/etc/apps/mltk-container/local/images.conf`로 이동하여 예를 들어 새 절:

```properties
[myimage]
title = My custom image
image = mltk-container-myimage
repo = your_local_docker_repo/
runtime = none,nvidia
```

## Further documentation and usage

Please find further information and documentation contained in the Deep Learning Toolkit app on splunkbase: Download and install the Deep Learning Toolkit
splunkbase에서 Deep Learning Toolkit 앱에 포함 된 추가 정보 및 문서를 찾으십시오. Deep Learning Toolkit을 다운로드하여 설치하십시오.