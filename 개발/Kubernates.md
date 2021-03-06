# ๐ฅKubernates
**์ปจํ์ด๋ ๊ฐ์ํ** ๊ธฐ์ ์ ์๋น์ค๊ฐ์ ์์๊ฒฉ๋ฆฌ๋ฅผ ํ๋๋ฐ OS๋ฅผ ๋ณ๋๋ก ์๋์๋ ๋๋ค.  
**OS ๊ธฐ๋์๊ฐ์ด ์๊ธฐ ๋๋ฌธ์** ์๋ํ์์ ์์ฒญ ๋น ๋ฅด๊ณ , ์์ ํจ์จ๋ ๋งค์ฐ ๋์ต๋๋ค.
> ์์ฒญ ๋ง์ ์ปจํ์ด๋, ์๋น์ค๋ฅผ ์ด์ ์ ์ด๋ฅผ ์ผ์ผ์ด ๋ฐฐํฌํ๊ณ  ์ด์ํ๋ ์ญํ ์ ํด์ฃผ๋๊ฒ **์ปจํ์ด๋ ์ค์ผ์คํธ๋ ์ดํฐ**๋ผ๋ ๊ฐ๋์ด๋ค. (์ฌ๋ฌ ์ปจํ์ด๋๋ฅผ ๊ด๋ฆฌํด์ฃผ๋ ์๋ฃจ์)  
๋ง์ ํด๋ผ์ฐ๋ ์๋น์ค ๊ธฐ์๋ค์ด ์ฟ ๋ฒ๋คํฐ์ค ํ๊ฒฝ์ด ์ค์น๋์ด ์๋ ์ธํ๋ผ๋ฅผ ์๋น์ค ํ๊ณ  ์๋ค.

# ๊ธฐ์  ์คํ
## USER
- Workloads
    - Pod
    - Replication Controller
    - ReplicaSet
    - StatefulSet
    - Deployment
    - DaemonSet
    - Job
    - CronJob
- Service
    - Service
    - Endpoints
    - Ingress
- Storage / Config
    - PV
    - PVC
    - ConfigMap
    - Secret
    - StorageClass
    - Volume
- Metadata
    - LimitRange
    - CRD
    - Event
    - HPA
    - MWC / VWC
    - PriorityClass
    - PodTemplate
    - PodDisruption
    - PodPreset
    - PodSecurity Policy
- Cluster
    - Namespace
    - ResourceQuota
    - Role / RoleBinding
    - Policy
    - ServiceAcoount
    - NodeScheduling
    - ...
## Admin
- Installation
- Architecture
- Networking
- Monitoring
- Plugins
- Ecosystem

## Kubernetes๋ฅผ ์ฌ์ฉํด์ผ ํ๋ ์ด์ ?
- **Auto Scaling**์ ๋ฐ๋ผ ํธ๋ํฝ์์ ๋ฐ๋ผ ์๋ฒ ์์์ ๋ฐ๊ฟ์ค๋ค.
- **Auto Healing** ์ฅ์ ๊ฐ ๋ฐ์ํ ์๋ฒ์์ ์๋ ์๋น์ค๋ค์ด ๋ค๋ฅธ ์์์ผ๋ก ๋ฐ๊ฟ์ ํ ๋นํด์ค๋ค.
- **Deployment** ๊ธฐ๋ฅ์ ๋ฐ๋ผ ํจ์จ์ ์ธ ๋ฐฐํฌ ๊ฐ๋ฅ
  
## VM vs Container
๋์ปค๋ ์ฌ๋ฌ ์ปจํ์ด๋๋ค๊ฐ์ ํธ์คํธ ์์์ ๋ถ๋ฆฌํด์ ์ฌ์ฉํ๊ฒ ํด์ค๋ค. ์ด๊ฒ์ ๋ฆฌ๋์ค ๊ณ ์ ๊ธฐ์ ์ธ name space์ cgroup ๋ฅผ ์ฌ์ฉํ์ฌ ๊ฒฉ๋ฆฌํ๋๊ฒ์ด๋ค.
- namespace : ์ปค๋์ ๊ด๋ จ ๋ ์์ญ์ ๋ถ๋ฆฌ
    - mnt, pid, net, ipc, uts, user
- cgroups : ์์์ ๋ํ ์์ญ์ ๋ถ๋ฆฌ
    - memory, CPU, I/O, network

![image](https://user-images.githubusercontent.com/71022555/165774963-294d9d54-b628-4dec-849e-0e58e021da85.png)
  
|Virtual Machine|Container|
|:---|:---|
|๊ฐ๊ฐ์ OS๋ฅผ ๊ณต์ |ํ OS๋ฅผ ๊ณต์ |
|๋ฆฌ๋์ค VM์์ Window ์ปจํ์ด๋ ์ฌ์ฉ ๊ฐ๋ฅ|๋ฆฌ๋์ค ์ปจํ์ด๋์์ Window์ฉ ์ปจํ์ด๋ ์ฌ์ฉ X|
|๋ณด์์  ์ธก๋ฉด ์ ๋ฆฌ|๋ณด์์  ์ธก๋ฉด ๋ถ๋ฆฌ|
|์ํ๋ Guest OS๋ฅผ ํตํด ์๋|ํ์ํ ์๋น์ค๊ฐ ๋์๊ฐ๋๋ฐ ํ์ํ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๊ฐ ์๋ ์ด๋ฏธ์ง๋น ๋ง๋ค์ด์ ๋์|