https://cloudowski.com/articles/10-differences-between-openshift-and-kubernetes/

If you’re interested in OpenShift 4 please check out also my honest review of it.

OpenShift has been often called as “Enterprise Kubernetes” by its vendor - Red Hat. In this article, I’m describing real differences between OpenShift and Kubernetes. It’s often confusing, as Red Hat tends to describe it as PaaS, sometimes hiding the fact that Kubernetes is an integral part of OpenShift with more features built around it. Let’s dive in and check what are the real differences between those two.

1. OpenShift product vs. Kubernetes project
Kubernetes is an open source project (or even a framework), while OpenShift is a product that comes in many variants. There’s an open source version of OpenShift which is called OKD. Previously it was called OpenShift Origin, but some “clever” folks at Red Hat came up with this new name which supposes to mean “The Origin Community Distribution of Kubernetes that powers Red Hat OpenShift” (?). But let’s forget about names for a while and focus on what are implications of that.

There are a couple of them:

OpenShift Container Platform is a product that you can install on your infrastructure that has a paid support included that comes with a subscription
You need to renew your OpenShift subscription for your cluster and you pay more when your cluster grows
Kubernetes has many distributions, but it’s a project and if something bad happens you can count mostly on community or external experts (in some cases they might be sometimes better than Red Hat support :-) )
Kubernetes has many releases per year (4 actually), OpenShift has also many releases, but it falls behind Kubernetes release schedule - version 3 at the time of writing was far behind (it includes Kubernetes 1.11 while the newest release was 1.14) while version 4 was “only” one release behind and should follow upstream Kubernetes in the future releases
As a product OpenShift subscription includes CloudForms (only in version 3) that enhance it with its features (e.g. configurable chargeback, monitoring, central provisioning etc.)
OKD version is free to use and includes most of the features of its commercial product, but you cannot buy a support nor you cannot use Red Hat based official images
So if you need a support for Kubernetes one option would be to buy subscription for OpenShift. If you’re okay with self-support then of course there’s Kubernetes with plenty of side projects, whole ecosystem and fantastic community. For hesitant ones there’s a OKD project with almost all OpenShift features - you can later might decide to migrate to a commercial product or stick with OKD.

Which is better?

It depends on whether you’d rather pay and use support and all the features that come with a product (OpenShift) rather than project (Kubernetes, but also OKD) with self-support model.

2. OpenShift limited installation vs. install Kubernetes (almost) anywhere
If you decide to install OpenShift you need to use either

Red Hat Enterprise Linux (RHEL) or Red Hat Atomic on OpenShift 3
Red Hat CoreOS (required by control plane - master and infra server, default for compute nodes) and optionally RHEL for compute nodes only on OpenShift 4
RHEL or CentOS for OKD.
You cannot install it on other linux distributions. Kubernetes, on the other hand, can be installed almost on any linux distribution such as Debian, Ubuntu (most popular ones) and many others.

When it comes to installation when choosing OpenShift you can install it on multiple platforms depending on the version:

OpenShift 3 - manually following reference guides (yes, you need to install it using ssh, yum, vim and other old-school tools) or with openshift-ansible project. The latter is probably a better choice, but since it needs to be universal and it’s written in Ansible it’s a little bit slow, complex and hard to troubleshoot. It does come with a major feature coveted by enterprise environments - rolling-update of the whole cluster. This is a major advantage and you will probably appreciate it when you decide to upgrade your Kubernetes cluster.
OpenShift 4 - has a simplified and easier to use installer that currently supports AWS and vSphere. It is performed by a dedicated Operator software and the whole configuration is kept in ConfigMaps inside a cluster (not in files on master servers like in version 3). Bare metal installations are still possible but currently they require many manual steps. Also it requires internet connections so disconnected installations are unavailable.
Kubernetes on the other hand has many installation tools available (e.g. kubeadm, kube-spray, kops), some of them are better for cloud, some are more universal and complex too and it’s up to you to decide how you want to install your cluster and upgrade it (if it’s supported by the tool).

The last thing regarding freedom of choice for your platform are services available on major cloud platforms. Kubernetes is available on three of them - GKE on Google GCP, EKS on Amazon AWS anf AKS on Microsoft Azure.
For OpenShift there’s a product called OpenShift Online, OpenShift Dedicatedand OpenShift on Azure. Additionally you can test your single node installations using the following methods:

Minikube for Kubernetes
Minishift for OKD (formerly OpenShift Origin)
CDK for OpenShift Container Platform 3
CRC (CodeReady Containers) for OpenShift Container Platform 4
Which is better?

Kubernetes has become a standard and is available on more platforms than OpenShift. However, with the new, more flexible and faster installer we can expect that OpenShift will be a good alternative for Kubernetes, also in the cloud.

3. OpenShift has more strict security policies than default Kubernetes
It’s probably because of the target group for OpenShift product, but indeed default policies are more strict there than on Kubernetes. For example, most of container images available on Docker Hub won’t run on OpenShift, as it forbids to run a container as root and even many of official images don’t meet this requirement. That’s why people are sometimes confused and angry because they cannot run simple apps like they used to on Kubernetes. There’s an easy way to disable that policy, but still it shows a different approach to security.

Also, RBAC was an integral part of OpenShift since many releases while there are some people who use Kubernetes without RBAC security. That’s okay for a small dev/test setup, but in real life, you want to have some level of permissions - even if it’s sometimes hard to learn and comprehend (because it is at first). In OpenShift you actually don’t have a choice and you have to use it and learn it on the way as you deploy more and more apps on it.

Last part is authentication and authorization model. There are additional mechanisms in OpenShift that makes integration with Active Directory easy, but more interesting part is authorization to external apps. As a part of OpenShift you can install additional component such as

Internal Container Registry
Logging stack based on EFK (ElasticSearch, Fluentd, Kibana)
Monitoring based on Prometheus
Jenkins
and you use a single account to authenticate to them with OAuth mechanism (oauth-proxies running as sidecars). That makes permissions management easier and can bring additional features like in EFK where you see logs only from namespaces/projects you have access to. And yes - you can achieve the same on Kubernetes as well, but it requires a lot of work.

Which is better?

Definitely “secure by default” approach in OpenShift.

4. OpenShift templates are less flexible than Kubernetes Helm charts
For someone coming straight from Kubernetes world who used Helm and its charts, OpenShift templates as the main method of deployment whole stack of resources is just too simple. Helm charts use sophisticated templates and package versioning that OpenShift templates are missing. It makes deployment harder on OpenShift and in most cases you need some external wrappers (like I do) to make it more flexible and useful in more complex scenarios than just simple, one pod application deployments. Helm is so much better, but its current architecture (Tiller component installed as Pod with huge permissions) isn’t compatible with more strict security polices in OpenShift.

There are some other options available in OpenShift 3 such as Automation Broker (formerly Ansible Service Broker) or Service Catalog, but they can be installed on Kubernetes while Helm is not a (supported) option on OpenShift. Hopefully, it will change in future with version 3 of Helm where there will be no Tiller component that makes it hard to make secure. Until then when working on OpenShift you need to live somehow with those inflexible templates looking with envy on those fancy Helm charts.

OpenShift 4 has an integrated OperatorHub which is becoming the preferred way for provisioning services (i.e. databases, queue systems). It will become eventually the best way to deploy services on OpenShift (and Kubernetes too).

Which is better?

Kubernetes Helm is more flexible and upcoming version 3 will make it more secure and applicable in more serious projects. However, with more operators available on OperatorHub, OpenShift 4 will gain an advantage.

5. Routers on OpenShift vs. Ingress on Kubernetes
Red Hat had needed an automated reverse proxy solution for containers running on OpenShift long before Kubernetes came up with Ingress. So now in OpenShift we have a Route objects which do almost the same job as Ingress in Kubernetes. The main difference is that routes are implemented by good, old HAproxy that can be replaced by commercial solution based on F5 BIG-IP. On Kubernetes, however, you have much more choice, as Ingress is an interface implemented by multiple servers starting from most popular nginx, traefik, AWS ELB/ALB, GCE, Kong and others including HAproxy as well.

So which one is better you may ask? Personally, I think HAproxy in OpenShift is much more mature, although doesn’t have as much features as some Ingress implementations. On Kubernetes however you can use different enhancements - my favorite one is an integration with cert-manager that allows you to automate management of SSL certificates. No more manual actions for issuing and renewal of certificates and additionally you can use trusted CA for free thanks to integration with Letsencrypt!

As an interesting fact, I want to mention that starting from OpenShift 3.10 Kubernetes Ingress objects are recognized by OpenShift and are translated/implemented by.. a router. It’s a big step towards compatibility with configuration prepared for Kubernetes that now can be launched on OpenShift without any modifications.

Which is better?

Both are great, Ingress is newer and less mature than Router, but they do a great job.

6. A different approach to deployments
Similarly like with Ingress, OpenShift chose to have a different way of managing deployments. In Kubernetes there are Deployment objects (you can also use them in OpenShift with all other Kubernetes objects as well) responsible for updating pods in a rolling update fashion and is implemented internally in controllers. OpenShift has a similar object called DeploymentConfig implemented not by controllers, but rather by sophisticated logic based on dedicated pods controlling whole process. It has some drawbacks, but also one significant advantage over Kubernetes Deployment - you can use hooks to prepare your environment for an update - e.g. by changing database schema. It’s a nifty feature that is hard to implement with Deployment (and no, InitContainers are not the same, as it’s hard to coordinate it with many instances running). Deployment, however, is better when dealing with multiple, concurrent updates - DeploymentConfig doesn’t support concurrent updates at all and in Kubernetes you can have many of them and it will manage to scale them properly.

Which is better?

OpenShift DeploymentConfig has more options and support ImageStream so I’m choosing it over classic Kubernetes Deployment.

7. Better management of container images
Now this is something that I really miss in Kubernetes and personally my favourite feature of OpenShift. ImageStreams for managing container images. Do you know how “easy” it is to change a tag for an image in a container registry? Without external tools such as skopeo you need to download the whole image, change it locally and push it back. Also promoting applications by changing container tags and updating Deployment object definition is not a pleasant way to do it.

That’s why I love ImageStreams and here are main reasons and features:

with ImageStream you upload a container image once and then you manage it’s virtual tags internally in OpenShift - in one project you would always use devel tag and only change reference to it internally, on prod you would use stable or prod tag and also manage it internally in OpenShift, not dealing with registry!
when using ImageStream with DeploymentConfig you can set a trigger which starts deployment when a new image appears or tag changes its reference - it’s perfect for development environments where app deploys itself whenever a new version is built (without any CICD process!)
with triggers you can achieve even more - having chained builds to create an updated version of your app/tool whenever a new version of the base image is published - it’s an automated patching of all container images built from insecure images at hand!
you can hide the origin of the image by exposing it as an ImageStream - e.g. one time jenkins points to a original, official image and when you wish to change something, you build your own, push it into your registry and change only reference in ImageStream, not in deployment configs like you would with traditional docker images
If you’re interested in more details you might want to check my article.

Which is better?

ImageStreams in OpenShift are awesome!

8. Integrated CI/CD with Jenkins
Red Hat created OpenShift long before Kubernetes project was found and from the start, it was a PaaS platform. By switching from their custom solution (they used something they called gears instead of containers) to Kubernetes it became easier to bring more features and one of the most exciting is integrated Jenkins. There are multiple CI/CD software solutions available, but Jenkins is still the biggest, most universal, generic and mature solution. It is also often used with Kubernetes clusters to build container images, perform Continuous Integration tasks on them and deploy them as containers on multiple environments with Continuous Deployment pipelines. Since it’s so popular then having it as a builtin part of OpenShift makes the whole CI/CD a lot less painful. Here’s a list of my favorite features of integrated Jenkins on OpenShift:

OAuth authentication - use your OpenShift login to log in to Jenkins and depending on the role you have on the project you get one of three jenkins role assigned (view, edit or admin). In OpenShift 4 it finally works as a Single-Sign-On (in version 3 you have to login to a service each time using the same credentials).
support for source-to-image that allows you to create a custom jenkins image - a few files with plugins list, custom configuration and other resources, enable you to easily update it when a source image changes (that part also can be automated!) and use Jekins in a fully “immutable” mode
two versions of template available - ephemeral for testing purposes and persistent with persistent storage for more serious job, configuration data and job history is kept separately from Jenkins itself and thus making it very easy to manage (e.g. upgrades, testing different sets of plugins)
synchronization of secret object from a namespace it’s running on - different secrets are synchronized with Jenkins credentials so that username/password, ssh key or secret text are available in your jobs without ever creating them in Jenkins
last but not least - pipeline definition is a separate BuildConfig object and is also defined outside of Jenkins as a OpenShift object from simple yaml file
Which is better?

Once again an additional feature of OpenShift makes it easy to deploy your apps with CI/CD pipelines.

9. OpenShift projects are more than Kubernetes namespaces
This a minor difference, but on OpenShift there are projects which are nothing more than just Kubernetes namespaces with additional features. Besides trivial things such as description and display name (trust me - they can be helpful when you have dozens of them), projects add some default objects. Currently a few roles (RoleBinding objects to be precise) are created alongside with a project, but you can modify default project template and use it to provision other objects. A good example would be network policies that close your project for external traffic so that is isolated and secure by default - if you want to permit some kind of traffic you would do so by creating additional policies explicitly. In a similar way you could provide default quotas or LimitRange objects and make your new projects pre-configured according to your organization rules.

Which is better?

Actually projects are namespaces with few features. There’s no clear winner here.

10. OpenShift is easier for beginners
As the last part I want to emphasize the difference when it comes to user experience. Containers are still new and having a complex, sophisticated interface for managing them makes it even harder to learn and adapt. That’s why I find OpenShift versions of both command line and web interfaces superior over Kubernetes ones.

Let’s start with cli. On OpenShift there’s a oc command which is equivalent of Kubernetes’ kubectl with the following differences:

oc has support for logging to OpenShift cluster - with kubectl you need to obtain your credentials and create your kubeconfig file with some external tools
oc lets you switch between projects/namespaces while kubectl doesn’t (you need exernal tools such as kubens/kubectx) - if you start working with many namespaces and clusters you will appreciate that feature, believe me
oc allows you to build a container image from a source and then deploy it onto environments with a single command! (oc new-app) It will create all required objects for you and then you may decide to export them, change and reapply or store somewhere in your repository
Let’s face it - if you’re beginner then you won’t touch command line at first - you’d probably choose to play with some web interface. And after you saw this

Kubernetes dashboard
Kubernetes dashboard
Kubernetes dashboard
Kubernetes dashboard screenshots
you would probably be discouraged as I did when I saw it for the first time (it was a couple of years ago, but it hasn’t changed a lot unfortunately). It can be overwhelming and personally I don’t use dashboard when working with Kubernetes, as it doesn’t bring much more information than command line and you are not able to change most of the things - it’s almost like read-only panel. Let’s face it - dashboard is not a first-class citizen among many Kubernetes projects.

Now here’s OpenShift 3 web console:

OpenShift 3 web console
OpenShift 3 web console
OpenShift 3 web console
OpenShift 3 web console
OpenShift 3 web console
OpenShift 3 web console
OpenShift 3 web console screenshots
And redesigned version available in OpenShift 4:

OpenShift 4 web console
OpenShift 4 web console
OpenShift 4 web console
OpenShift 4 web console
OpenShift 4 web console
OpenShift 4 web console
OpenShift 4 web console
OpenShift 4 web console screenshots
Now I’m not saying it’s the best web interface, but I consider it as one of the best features of OpenShift. First of all it has a login window, something that simple and trivial and I know it shouldn’t be a feature, but have you seen Kubernetes “login window”? Dashboard has a login window where you provide a token and honestly is confusing, especially for beginners. Most of all OpenShift web console is very useful, much more than Kubernetes dashboard. In fact, you can perform about 80% (or even 90% in OpenShift 4) of tasks directly from it - no need to launch command line or dealing with yaml objects - it can be actually a primary tool for managing OpenShift on a daily basis.

Which is better?

OpenShift. Sorry Kubernetes, but people (including me!) love and need fancy web console :-)

Conclusion
Some of you may think I’m a total OpenShift fanboy, but in reality, I love working with both - OpenShift and Kubernetes. After all they make it possible to deploy and manage our containerized apps in a way that was only available for unicorns like Google. So whichever you choose you’ll get tons of features making your life easier and your journey will begin towards cloud native world.


OpenShift 4에 관심이 있으시면 정직한 검토 도 확인 하십시오 .

OpenShift는 공급 업체 인 Red Hat에 의해 종종 "Enterprise Kubernetes"로 불립니다. 이 기사에서는 OpenShift와 Kubernetes의 실제 차이점을 설명합니다. Red Hat이이를 PaaS로 묘사하는 경향이 있기 때문에 종종 혼란 스럽습니다. 때로는 Kubernetes가 더 많은 기능을 갖춘 OpenShift의 필수 요소 라는 사실을 숨기고 있습니다. 이 두 가지의 실제 차이점이 무엇인지 살펴보고 확인하십시오.

1. OpenShift 제품과 Kubernetes 프로젝트
Kubernetes는 오픈 소스 프로젝트 (또는 프레임 워크)이며 OpenShift는 다양한 변형 이있는 제품 입니다. OKD 라는 OpenShift의 오픈 소스 버전이 있습니다. 이전에는 OpenShift Origin이라고 불렸지만 Red Hat의 일부 "영리한" 사람들은 "Red Hat OpenShift를 지원하는 Kubernetes의 Origin Community Distribution" (?) 을 의미하는 새로운 이름을 사용했습니다 . 그러나 잠시 동안의 이름을 잊어 버리고 그 의미에 초점을 맞추십시오.

그들 중 몇 가지가 있습니다 :

OpenShift Container Platform 은 서브 스크립 션과 함께 유료 지원이 포함 된 인프라에 설치할 수있는 제품입니다.
클러스터에 대한 OpenShift 구독을 갱신해야하며 클러스터가 커지면 더 많은 비용을 지불합니다
Kubernetes는 많은 배포판을 가지고 있지만 프로젝트이지만 잘못된 일이 발생하면 대부분 커뮤니티 또는 외부 전문가에게 의지 할 수 있습니다 (경우에 따라 때로는 Red Hat 지원보다 낫습니다 :-))
버전 -는 Kubernetes은 연간 많은 자료를 (4 실제로), OpenShift는 또한 많은 자료를 가지고 있지만이는 Kubernetes의 출시 일정 쳐가 3 버전하면서 글을 쓰는 시점 (최신 버전은 1.14하면서는 Kubernetes 1.11 포함) 뒤쳐져 있었다 4 하나의 릴리스 뒤에 "단지"였으며 이후 릴리스에서 업스트림 Kubernetes를 따라야합니다.
제품 OpenShift 구독에는 기능 (예 : 구성 가능한 지불 거절, 모니터링, 중앙 프로비저닝 등)으로 기능을 향상시키는 CloudForms (버전 3 만)가 포함됩니다.
OKD 버전은 무료이며 상용 제품의 대부분의 기능을 포함하지만 지원을 구매할 수 없으며 Red Hat 기반 공식 이미지를 사용할 수 없습니다
따라서 Kubernetes에 대한 지원이 필요한 경우 OpenShift 구독을 구매하는 것이 하나의 옵션입니다. 자체 지원에 만족한다면 물론 많은 보조 프로젝트, 전체 생태계 및 환상적인 커뮤니티가있는 Kubernetes가 있습니다. 망설이는 사람에게는 거의 모든 OpenShift 기능을 갖춘 OKD 프로젝트가 있습니다. 나중에 상용 제품으로 마이그레이션하거나 OKD를 고수하기로 결정할 수 있습니다.

어떤게 더 좋아?

자체 지원 모델이있는 프로젝트 (Kubernetes 및 OKD)가 아닌 제품 (OpenShift)과 함께 제공되는 모든 기능과 지원을 지불하고 사용할지 여부에 따라 다릅니다.

2. OpenShift 제한 설치 대 Kubernetes 설치 (거의)
OpenShift를 설치하기로 결정한 경우 다음 중 하나를 사용해야합니다.

OpenShift 3의 Red Hat Enterprise Linux (RHEL) 또는 Red Hat Atomic
Red Hat CoreOS (제어 평면에 필요함-마스터 및 인프라 서버, 컴퓨팅 노드의 기본값) 및 OpenShift 4에서만 컴퓨팅 노드의 경우 선택적으로 RHEL
OKD 용 RHEL 또는 CentOS.
당신은 할 수없는 다른 리눅스 배포판에 설치합니다. 반면 Kubernetes는 데비안, 우분투 (가장 인기있는 것) 및 기타 많은 리눅스 배포판에 거의 설치할 수 있습니다.

OpenShift를 선택할 때 설치와 관련하여 버전에 따라 여러 플랫폼에 설치할 수 있습니다.

OpenShift 3-수동으로 참조 가이드를 따르거나 (예, ssh, yum, vim 및 기타 구식 도구를 사용하여 설치해야 함) 오픈 시프트 가능 프로젝트 와 함께 설치해야 합니다. 후자는 아마도 더 나은 선택 일 수 있지만 보편적이어야하고 Ansible로 작성되었으므로 약간 느리고 복잡하며 문제 해결이 어렵습니다. 전체 클러스터의 롤링 업데이트 -엔터프라이즈 환경에서 제공하는 주요 기능이 제공됩니다 . 이것은 주요 이점이며 Kubernetes 클러스터를 업그레이드하기로 결정할 때이 점을 높이 평가할 것입니다.
OpenShift 4-현재 AWS 및 vSphere를 지원하는 간단하고 사용하기 쉬운 설치 관리자가 있습니다. 전용 운영자 소프트웨어에 의해 수행되며 전체 구성은 클러스터 내의 ConfigMaps (버전 3과 같은 마스터 서버의 파일이 아님)에 보관됩니다. 베어 메탈 설치는 여전히 가능하지만 현재는 많은 수동 단계가 필요합니다. 또한 인터넷 연결이 필요하므로 연결이 끊긴 설치를 사용할 수 없습니다 .
반면 Kubernetes에는 사용 가능한 많은 설치 도구가 있습니다 (예 : kubeadm, kube-spray, kops). 일부는 클라우드에 적합하고 일부는 더 보편적이며 복잡하며 클러스터 설치 방법을 결정하는 것은 사용자의 책임입니다. 도구에서 지원하는 경우 업그레이드하십시오.

플랫폼 선택의 자유에 관한 마지막 것은 주요 클라우드 플랫폼에서 사용할 수있는 서비스입니다. Kubernetes는 Google GCP의 GKE, Amazon AWS의 EKS, Microsoft Azure의 AKS 등 세 가지로 제공됩니다.
OpenShift의 경우 OpenShift Online , OpenShift Dedicated 및 OpenShift on Azure 제품이 있습니다. 또한 다음 방법을 사용하여 단일 노드 설치를 테스트 할 수 있습니다.

쿠 버네 티스 용 미니 쿠베
OKD 용 미니 시프트 (이전의 OpenShift Origin)
OpenShift 컨테이너 플랫폼 3 용 CDK
OpenShift 컨테이너 플랫폼 4 용 CRC (CodeReady 컨테이너)
어떤게 더 좋아?

Kubernetes는 표준이되었으며 OpenShift보다 더 많은 플랫폼에서 사용할 수 있습니다. 그러나보다 유연하고 빠른 새 설치 프로그램을 사용하면 OpenShift가 클라우드에서도 Kubernetes를 대체 할 수있는 좋은 대안이 될 것입니다.

3. OpenShift는 기본 Kubernetes보다 더 엄격한 보안 정책을 가지고 있습니다
OpenShift 제품의 대상 그룹 때문일 수 있지만 Kubernetes보다 기본 정책이 더 엄격합니다. 예를 들어 Docker Hub에서 사용할 수있는 대부분의 컨테이너 이미지 는 컨테이너를 루트로 실행하는 것을 금지하고 많은 공식 이미지도이 요구 사항을 충족하지 않기 때문에 OpenShift에서 실행 되지 않습니다. 그렇기 때문에 사람들은 Kubernetes에서 사용했던 것처럼 간단한 앱을 실행할 수 없기 때문에 때때로 혼란스럽고 화를냅니다. 해당 정책을 사용하지 않도록 설정하는 쉬운 방법이 있지만 여전히 보안에 대한 다른 접근 방식을 보여줍니다.

또한 RBAC 보안이없는 Kubernetes를 사용하는 사람들이 많은 반면 RBAC는 많은 릴리스 이후 OpenShift의 핵심 부분이었습니다. 소규모 dev / test 설정에는 문제가 없지만 실제로는 배우고 이해하기 어려운 경우에도 (처음에 있기 때문에) 어느 정도의 권한이 필요합니다. OpenShift에서는 실제로 선택의 여지가 없으며 더 많은 앱을 배포 할 때이를 선택하고 학습해야합니다.

마지막 부분은 인증 및 권한 부여 모델입니다. OpenShift에는 Active Directory와 쉽게 통합 할 수있는 추가 메커니즘이 있지만 더 흥미로운 부분은 외부 앱에 대한 인증입니다. OpenShift의 일부로 다음과 같은 추가 구성 요소를 설치할 수 있습니다

내부 컨테이너 레지스트리
EFK (ElasticSearch, Fluentd, Kibana)를 기반으로하는 로깅 스택
Prometheus 기반 모니터링
젠킨스
단일 계정을 사용하여 OAuth 메커니즘 (사이드카로 실행되는 oauth-proxy)으로 인증합니다. 따라서 권한 관리가 쉬워지고 EFK와 같이 액세스 할 수있는 네임 스페이스 / 프로젝트에서만 로그를 볼 수있는 추가 기능을 제공 할 수 있습니다. 그리고 예-Kubernetes에서도 동일하게 달성 할 수 있지만 많은 작업이 필요합니다.

어떤게 더 좋아?

OpenShift의 확실한 "기본 보안"접근 방식.

4. OpenShift 템플릿은 Kubernetes Helm 차트보다 유연성이 떨어집니다.
Helm과 그 차트를 사용한 Kubernetes 세계에서 직접 온 누군가에게 OpenShift 템플릿은 전체 리소스 스택을 배포하는 주요 방법으로 사용하기가 너무 간단합니다. Helm 차트는 OpenShift 템플릿이없는 정교한 템플릿 및 패키지 버전 관리를 사용합니다. OpenShift에서 배포가 더 어려워지며 대부분의 경우 간단한 포드 응용 프로그램 배포보다 복잡한 시나리오에서보다 유연하고 유용하게 만들려면 외부 랩퍼가 필요합니다. Helm은 훨씬 나아지 지만 현재 아키텍처 (큰 권한으로 포드로 설치된 Tiller 구성 요소)는 OpenShift의보다 엄격한 보안 정책과 호환되지 않습니다.

OpenShift 3에는 Automation Broker (이전의 Ansible Service Broker) 또는 Service Catalog와 같은 다른 옵션이 있지만 Helm이 OpenShift에서 지원되지 않는 동안 Kubernetes에 설치할 수 있습니다. 앞으로는 Helm 버전 3을 통해 보안을 강화하기 어려운 Tiller 구성 요소가 없을 것으로 예상됩니다. 그때까지 OpenShift에서 작업 할 때 멋진 Helm 차트에서 부러워하는 유연한 템플릿으로 살아야합니다.

OpenShift 4에는 통합 된 OperatorHub 가있어 서비스 (예 : 데이터베이스, 대기열 시스템)를 프로비저닝하는 데 선호되는 방법이되었습니다. 결국 OpenShift (및 Kubernetes)에 서비스를 배포하는 가장 좋은 방법이 될 것입니다.

어떤게 더 좋아?

Kubernetes Helm은 더욱 유연하며 향후 버전 3은보다 안전하고보다 심각한 프로젝트에 적용 할 수 있습니다. 그러나 OperatorHub에서 더 많은 연산자를 사용할 수 있으면 OpenShift 4가 유리합니다.

5. OpenShift의 라우터 대 Kubernetes의 입구
Kubernetes가 Ingress를 출시하기 훨씬 전에 Red Hat은 OpenShift에서 실행되는 컨테이너에 대한 자동 리버스 프록시 솔루션이 필요했습니다. 이제 OpenShift 에는 Kubernetes의 Ingress 와 거의 동일한 작업을 수행 하는 Route 객체가 있습니다. 가장 큰 차이점은 F5 BIG-IP를 기반으로하는 상용 솔루션으로 대체 할 수있는 오래된 구형 HAproxy로 경로를 구현한다는 것입니다. 그러나 Kubernetes에서는 Ingress가 가장 인기있는 nginx, traefik, AWS ELB / ALB, GCE, Kong 및 HAproxy를 포함한 다른 서버에서 시작하는 여러 서버로 구현되는 인터페이스이므로 훨씬 더 많은 선택권이 있습니다.

그렇다면 어느 쪽이 더 좋을까요? 개인적으로 OpenShift의 HAproxy는 훨씬 더 성숙하지만 일부 Ingress 구현만큼 많은 기능을 가지고 있지는 않습니다. 그러나 Kubernetes에서는 다른 개선 사항을 사용할 수 있습니다. 제가 가장 좋아하는 것은 SSL 인증서 관리를 자동화 할 수 있는 cert-manager 와의 통합입니다 . 인증서 발급 및 갱신에 대한 수동 작업이 더 이상 필요 없으며 Letsencrypt와의 통합 덕분에 신뢰할 수있는 CA를 무료로 사용할 수 있습니다 !

흥미로운 사실로 OpenShift 3.10부터 Kubernetes Ingress 객체는 OpenShift에 의해 인식되고 라우터에 의해 번역 / 구현됩니다. Kubernetes 용으로 준비된 구성과의 호환성을 향한 큰 단계입니다. 이제 수정없이 OpenShift에서 시작할 수 있습니다.

어떤게 더 좋아?

둘 다 훌륭하고 Ingress는 라우터보다 새롭고 덜 성숙하지만 훌륭한 일을합니다.

6. 다른 배포 방식
Ingress와 마찬가지로 OpenShift는 다른 배포 관리 방법을 선택했습니다. Kubernetes에는 Deployment롤링 업데이트 방식으로 포드를 업데이트하는 개체가 있으며 (OpenShift에서 다른 모든 Kubernetes 개체와 함께 사용할 수도 있음) 컨트롤러 내부에서 구현됩니다. OpenShift에는 DeploymentConfig컨트롤러가 아닌 전체 프로세스를 제어하는 ​​전용 포드를 기반으로 한 정교한 논리에 의해 구현 되는 유사한 객체가 있습니다. 몇 가지 단점이 있지만 Kubernetes 배포에 비해 중요한 이점도 있습니다. 후크 를 사용할 수 있습니다업데이트를 위해 환경을 준비합니다 (예 : 데이터베이스 스키마 변경). 배포와 함께 구현하기 어려운 멋진 기능입니다 (InitContainers는 실행중인 많은 인스턴스와 조정하기가 어렵 기 때문에 동일하지 않습니다). 그러나 여러 개의 동시 업데이트를 처리 할 때는 배포가 더 좋습니다. DeploymentConfig는 동시 업데이트를 전혀 지원하지 않으며 Kubernetes에서는 많은 업데이트를 보유 할 수 있으며이를 적절히 확장 할 수 있습니다.

어떤게 더 좋아?

OpenShift DeploymentConfig에는 더 많은 옵션이 있으며 ImageStream을 지원하므로 클래식 Kubernetes 배포에서 선택합니다.

7. 컨테이너 이미지 관리 개선
이제 이것은 Kubernetes에서 개인적으로 가장 그리워하는 부분이며 개인적으로 가장 좋아하는 OpenShift 기능입니다. ImageStreams컨테이너 이미지를 관리합니다. 컨테이너 레지스트리에서 이미지의 태그를 변경하는 것이 얼마나 쉬운 지 아십니까? skopeo 와 같은 외부 도구가 없으면 전체 이미지를 다운로드하고 로컬로 변경 한 후 다시 밀어야합니다. 또한 컨테이너 태그를 변경하고 배포 개체 정의를 업데이트하여 응용 프로그램을 홍보하는 것은 즐거운 방법이 아닙니다.

이것이 ImageStreams를 좋아하는 이유이며 주요 이유와 특징은 다음과 같습니다.

ImageStream을 사용하면 컨테이너 이미지를 한 번 업로드 한 다음 OpenShift에서 내부적으로 가상 태그 를 관리합니다 . 한 프로젝트에서는 항상 devel 태그를 사용 하고 내부적으로 참조를 변경합니다. 안정적으로 또는 prod 태그를 사용하고 내부적으로도 관리합니다 레지스트리를 다루지 않는 OpenShift에서!
DeploymentConfig 와 함께 ImageStream 을 사용 하면 새 이미지가 나타나거나 참조가 태그 변경 될 때 배포를 시작하는 트리거를 설정할 수 있습니다. CICD 프로세스없이 새 버전이 빌드 ​​될 때마다 앱이 자체적으로 배포되는 개발 환경에 적합합니다!
트리거 를 사용하면 기본 이미지의 새 버전이 게시 될 때마다 체인 된 빌드 로 앱 / 도구의 업데이트 된 버전을 만들 수 있습니다. 안전하지 않은 이미지로 작성된 모든 컨테이너 이미지의 자동 패치입니다!
이미지 원본을 ImageStream으로 노출하여 이미지의 출처를 숨길 수 있습니다. 예를 들어 jenkins 가 원본의 공식 이미지를 가리키고 무언가를 변경하고 싶을 때는 직접 빌드하고 레지스트리로 푸시 한 다음 ImageStream의 참조 만 변경하십시오. , 기존 도커 이미지와 같은 배포 구성이 아닌
더 자세한 정보가 필요하면 내 기사 를 확인 하십시오 .

어떤게 더 좋아?

OpenShift의 ImageStream은 훌륭합니다!

8. Jenkins와 통합 된 CI / CD
Red Hat은 Kubernetes 프로젝트가 발견되기 훨씬 전에 OpenShift를 만들었으며 처음부터 PaaS 플랫폼이었습니다. 맞춤형 솔루션에서 전환함으로써 (그들은 기어 라고 불리는 것을 사용했습니다)컨테이너 대신) Kubernetes에 더 많은 기능을 제공하는 것이 쉬워졌으며 가장 흥미로운 것은 통합 Jenkins입니다. 사용 가능한 CI / CD 소프트웨어 솔루션은 여러 가지가 있지만 Jenkins는 여전히 가장 크고 가장 보편적이며 일반적이며 성숙한 솔루션입니다. 또한 Kubernetes 클러스터와 함께 컨테이너 이미지를 작성하고 Continuous Integration 작업을 수행하며 Continuous Deployment 파이프 라인을 사용하여 여러 환경에 컨테이너로 배포하는 데 종종 사용됩니다. 매우 인기가 높기 때문에 OpenShift의 내장 부분으로 사용하면 전체 CI / CD를 훨씬 덜 고통스럽게 만듭니다. OpenShift에서 통합 Jenkins의 가장 좋아하는 기능 목록은 다음과 같습니다.

OAuth 인증-OpenShift 로그인을 사용하여 Jenkins에 로그인하고 프로젝트에 대한 역할에 따라 3 가지 jenkins 역할 중 하나 (보기, 편집 또는 관리)가 부여됩니다. OpenShift 4에서는 최종적으로 싱글 사인온 (SSO)으로 작동합니다 (버전 3에서는 동일한 자격 증명을 사용하여 매번 서비스에 로그인해야 함).
플러그인 목록, 사용자 정의 구성 및 기타 리소스가 포함 된 일부 파일 인 사용자 정의 jenkins 이미지를 생성 할 수있는 소스 간 이미지 지원-소스 이미지가 변경 될 때 쉽게 해당 이미지를 업데이트 할 수 있습니다 (해당 부분도 자동화 가능). 완전히 "불변" 모드 에서 Jekins를 사용 합니다
두 가지 버전의 템플릿 사용 가능- 테스트 목적으로 임시적 이며 보다 심각한 작업을 위해 영구 스토리지로 지속적 으로 유지, 구성 데이터 및 작업 기록은 Jenkins 자체와 별도로 유지되므로 관리가 매우 쉽습니다 (예 : 업그레이드, 다양한 플러그인 세트 테스트)
실행중인 네임 스페이스에서 비밀 개체 동기화-다른 비밀은 Jenkins 자격 증명과 동기화되므로 Jenkins에서 사용자 이름 / 암호, ssh 키 또는 비밀 텍스트를 만들지 않고도 작업에서 사용할 수 있습니다.
마지막으로 파이프 라인 정의는 별도의 BuildConfig 객체이며 Jenkins 외부에서도 간단한 yaml 파일의 OpenShift 객체로 정의됩니다.
어떤게 더 좋아?

OpenShift의 추가 기능을 사용하면 CI / CD 파이프 라인을 사용하여 앱을 쉽게 배포 할 수 있습니다.

9. OpenShift 프로젝트는 Kubernetes 네임 스페이스 이상입니다.
이것은 사소한 차이이지만 OpenShift에는 추가 기능 이있는 Kubernetes 네임 스페이스에 지나지 않는 프로젝트 가 있습니다. 설명 및 표시 이름과 같은 사소한 것 외에도 (수십 개가 있으면 도움이 될 수 있습니다) 프로젝트에는 기본 객체가 추가됩니다. 현재 몇 개의 역할 ( 정확한 객체)이 프로젝트와 함께 생성되지만 기본 프로젝트 템플릿을 수정하고이를 사용하여 다른 객체를 프로비저닝 할 수 있습니다. 좋은 예로는 외부 트래픽에 대해 프로젝트를 닫아 기본적으로 격리되고 안전한 네트워크 정책 을들 수 있습니다. 어떤 종류의 트래픽을 허용하려면 추가 정책을 명시 적으로 작성하면됩니다. 비슷한 방법으로 기본 할당량을 제공하거나RoleBindingLimitRange 조직 규칙에 따라 새 프로젝트를 사전 구성합니다.

어떤게 더 좋아?

실제로 프로젝트는 기능이 거의없는 네임 스페이스입니다. 확실한 승자는 없습니다.

10. 초보자에게는 OpenShift가 더 쉽습니다.
마지막으로 사용자 경험과 관련하여 차이점을 강조하고 싶습니다. 컨테이너는 여전히 새롭고 관리하기위한 복잡하고 정교한 인터페이스를 통해 배우고 적응하기가 더욱 어려워집니다. 이것이 커맨드 라인과 웹 인터페이스의 OpenShift 버전이 Kubernetes보다 우수한 이유입니다.

cli로 시작하겠습니다. OpenShift에는 ocKubernetes kubectl와 동등한 명령이 있으며 다음과 같은 차이점이 있습니다.

ocOpenShift 클러스터에 로깅을 지원합니다-kubectl을 사용하면 자격 증명을 얻고 kubeconfig외부 도구를 사용하여 파일을 만들어야합니다
oc프로젝트 / 네임 스페이스 사이를 전환 kubectl하지 않아도 ( kubens / kubectx 와 같은 외부 도구가 필요함 )-많은 네임 스페이스 및 클러스터로 작업을 시작하면 그 기능을 높이 평가할 것입니다.
oc소스에서 컨테이너 이미지를 빌드 한 다음 단일 명령 으로 환경에 배치 할 수 있습니다 ! ( oc new-app) 필요한 모든 객체 를 생성 한 다음 해당 객체 를 내보내거나 변경 및 재 적용하거나 저장소에 저장할 수 있습니다.
초보자라면 처음에는 명령 줄을 건드리지 않을 것입니다. 아마도 웹 인터페이스를 가지고 노는 것을 선택했을 것입니다. 그리고 당신이 이것을 본 후에

Kubernetes 대시 보드
Kubernetes 대시 보드
Kubernetes 대시 보드
Kubernetes 대시 보드 스크린 샷
처음 보았을 때와 같이 실망했을 것입니다 (2 년 전이지만 불행히도 많이 바뀌지 않았습니다). Kubernetes로 작업 할 때 대시 보드를 사용하지 않는 것은 압도적이며 개인적으로 명령 줄보다 많은 정보를 제공하지 않으며 대부분의 것을 변경할 수 없으므로 읽기 전용 패널과 거의 같습니다. 대시 보드는 많은 Kubernetes 프로젝트에서 일류 시민 이 아닙니다 .

이제 OpenShift 3 웹 콘솔이 있습니다 :

OpenShift 3 웹 콘솔
OpenShift 3 웹 콘솔
OpenShift 3 웹 콘솔
OpenShift 3 웹 콘솔
OpenShift 3 웹 콘솔
OpenShift 3 웹 콘솔
OpenShift 3 웹 콘솔 스크린 샷
그리고 OpenShift 4에서 사용 가능한 재 설계된 버전 :

OpenShift 4 웹 콘솔
OpenShift 4 웹 콘솔
OpenShift 4 웹 콘솔
OpenShift 4 웹 콘솔
OpenShift 4 웹 콘솔
OpenShift 4 웹 콘솔
OpenShift 4 웹 콘솔
OpenShift 4 웹 콘솔 스크린 샷
이제는 이것이 최고의 웹 인터페이스라고 말하지는 않지만 OpenShift의 최고의 기능 중 하나로 간주합니다. 우선 로그인 창, 간단하고 사소한 것이 있으며 기능이 아니어야한다는 것을 알고 있지만 Kubernetes의 "로그인 창"을 보셨습니까? 대시 보드에는 토큰을 제공하는 로그인 창이 있으며 특히 초보자에게는 혼란 스럽습니다. 대부분의 OpenShift 웹 콘솔은 Kubernetes 대시 보드보다 훨씬 유용합니다 . 실제로 명령 줄을 시작하거나 yaml 개체를 처리 할 필요없이 약 80 % (또는 OpenShift 4에서 90 %)의 작업을 직접 수행 할 수 있습니다. 실제로 매일 OpenShift를 관리하는 기본 도구가 될 수 있습니다. .

어떤게 더 좋아?

OpenShift. 죄송합니다. Kubernetes이지만 사람들 (나 포함)은 멋진 웹 콘솔을 좋아하고 필요합니다 :-)

결론
여러분 중 일부는 제가 총 OpenShift 팬보이라고 생각할 수도 있지만 실제로는 OpenShift와 Kubernetes와 함께 일하는 것을 좋아합니다. 결국 그들은 Google과 같은 유니콘에서만 사용할 수있는 방식으로 컨테이너화 된 앱을 배포하고 관리 할 수 ​​있습니다. 어느 쪽을 선택하든 인생을 편하게 해주는 수많은 기능을 이용할 수 있으며 여행은 클라우드 네이티브 세계로 향할 것입니다.