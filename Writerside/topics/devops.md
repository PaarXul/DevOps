# DevOps

 Necesito un grafico que incluya los siguientes elementos:
 
 - el ciclo de vida de DevOps con las siguientes fases:
    - Plan
    - Code
    - Build
    - Test
    - Release
    - Deploy
    - Operate
    - Monitor

- Herramientas de DevOps:
  - Jenkins
  - SonarQube
  - Sonatype
  - git
  - Maven
  - Slack

Diagrama de flujo de DevOps con las herramientas mencionadas anteriormente


```mermaid
flowchart LR
    A[DevOps] -->|Cicle live| B(Plan)
    B ---> z1(JIRA)
    B ---> z2(Concluence)
    B --> |Step2| C[Code]
    C ---> y1[GitHub]
    C ---> y2[GitLab]
    C ---> y3[Azure]
    C --> |Step3| D(Build)
    D ---> x1[Maven]
    D ---> x2[Gradle]
    D ---> x3[NPM]
    D ---> x4[Webpack]
    D --> |Step4| E(Test)
    E ---> w1[Junit]
    E ---> w2[PlayWright]
    E ---> w3[Jest]
    E ---> |Step5| F(Release)
    F ---> q1[Jenkins]
    F ---> q2[BuildKite]
    F --> |Step6| H(Deploy)
    H ---> v1[Docker]
    H ---> v2[Argo]
    H ---> v3[AWS Lamda]
    H --> |Step7| I(Operate)
    I ---> r1[Kubernets]
    I ---> r2[Terraform]
    I --> |Step 8| J(Monitor)
    J ---> s1[Prometeus]
    J ---> s2[DataDog]
    J ---> s3[Grafana]
    J ---> |Cicle| B


```

