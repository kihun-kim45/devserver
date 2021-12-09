# Contents

1. 주요 업무

   - 1.1. 업무 내용
   - 1.2. 업무 진행상황 및 주요 내용

2. 업무 프로세스

   - 2.1. 업무 개요

     - 2.1.1. 업무 리스트 및 설명

   - 2.2 업무
     - 2.2.1. 개발 환경
     - 2.2.2. 사용 방법
       - 2.2.2.1. 환경 설정
       - 2.2.2.2. 배포 & 설치
       - 2.2.2.3. 적용
       - 2.2.2.4. 평가

# 1. 주요 업무

## 1.1. 업무 내용

- uds.ai 개발 및 유지보수

## 1.2. 업무 진행상황 및 주요내용

- uds.ai 개발

  - Home
  - Solutions (SBMS, Udap, QMS)
  - Blog
  - About
  - Recruit
  - Contact

* Grafana plugin 개발

  - Panel

    - Trend-panel
    - Tabulator-panel
    - Wafermap-panel(grafana-plotly-panel)
    - plotly-panel(CP,CPK,SL)

  - Datasource
    - simple-json-datasource
    - Influx-datasource
    - trend-datasource
    - measurement-datasource

# 2. 업무 프로세스

## 2.1. 업무 개요

### 2.1.1. 업무리스트 및 설명

| 업무      |        세부 내용         | 비고 |
| --------- | :----------------------: | :--: |
| `Grafana` | grafana 개발 및 유지보수 |      |

### 2.1.2. 개발 환경

| 분류                 |                        이름                        |   비고   |
| -------------------- | :------------------------------------------------: | :------: |
| `OS`                 |                   Windows 10 Pro                   |          |
| `Visual Studio Code` |                       Latest                       |          |
| `개발 언어`          |               Typescript, Javascript               | Frontend |
| `DOM framework`      | AngularJS(1.x version), reactjs, Jquery, VanillaJS | Frontend |
| `빌드 툴`            |                   Grunt, Webpack                   | Frontend |
| `개발 언어`          |                       Golang                       | Backend  |
| `WebFramework`       |                      macaron                       | Backend  |
| `빌드 툴`            |                        bra                         | Backend  |
| `database`           |                  sqlite, influxdb                  | Backend  |

- build Frontend

```
grafana
│
└───grafana
    │   ...
    └───conf                     # grafana config 설정 폴더
    │
    └───data                     # grafana의 데이터 및 plugin
    │
    └───dist                     # package build시 산출물 경로
    │
    └───pkg                      # backend source code
    │
    └───public                   # frontend source code
    │
    └───scripts/webpack          # frontend build 폴더
    │
    └───Dockerfile               # docker config file
    │
    └───package.json             #프로젝트의 의존성 관리 파일
    │
    └───.babelrc.json            # babel 설정 파일
    │
    └───tsconfig.json            # typescript 설정 파일
```

#### Frontend build

```bash
yarn --purelockfile
yarn watch
```

- 빌드 결과물 확인(Frontend)<br>
  ![image](https://user-images.githubusercontent.com/23473356/86866552-243d8000-c10c-11ea-91fa-667b7f2b6414.png)

#### Backend build

```
bra run # build backend + live reoload
```

- 빌드 결과물 확인(Backend)<br>
  ![image](https://user-images.githubusercontent.com/23473356/88517582-02a82800-d02a-11ea-89d1-a3ae731ccde7.png)

* 사이트 접속 확인(http://localhost:80)<br>
  ![image](https://user-images.githubusercontent.com/23473356/88517721-44d16980-d02a-11ea-8dc8-a985d12e6331.png)

#### Plugin Build

```
  yarn
  yarn dev
```

- 빌드 결과물 확인(Plugin)<br>
  ![image](https://user-images.githubusercontent.com/23473356/88520059-0473ea80-d02e-11ea-9413-a63f5d26ba6e.png)

# 3. 기능 설계

## 3.1 개발 개요

### 3.1.1 기능 리스트 및 설명

| 기능    |         세부내용          | 비고  |
| ------- | :-----------------------: | :---: |
| `기능1` |          grafana          | blank |
| `기능2` |   grafana-plugin-panel    | blank |
| `기능3` | grafana-plugin-datasource | blank |

## 3.2 기능

### 3.2.1 기능 개요

#### 3.2.1.1 기능 조건 및 대상

- grafana: 시각화 할 데이터의 api(eg. influxdb, python server api)
- grafana-plugin-panel: grafana-plugin-datasource
- grafana-plugin-datasource: (api server running 상태)

#### 3.2.1.2 시퀀스 다이어그램

![image](https://user-images.githubusercontent.com/23473356/89163433-8759ef80-d5b0-11ea-9550-f3a9c798169a.png)

### 3.2.2 클래스 및 함수

#### 3.2.2.1 주요 클래스 설명

| 클래스명                 |                                       설명                                        |
| ------------------------ | :-------------------------------------------------------------------------------: |
| `TrendPanelCtrl`         |     grafana-trend-panel의 main으로 tabulator와 plotly를 이용한 시각화를 총괄      |
| `GraphCtrl`              |       Plotly lib directive의 controller로 Graph 시각화와 라이프 사이클 관리       |
| `LegendCtrl`             |        Legend directive의 Controller Graph내의 시각화와 라이프 사이클 관리        |
| `MetricsTab`             |       Metricstab(query) directive의 Controller Panel내의 metric query 관리        |
| `TabulatorCtrl`          |        Tabulator directive의 Controller table 시각화와 라이프 사이클 관리         |
| `TooltipCtrl`            |  TooltipCtrl directive의 Controller graph의 tooltip 시각화와 라이프 사이클 관리   |
| `VariableCtrl`           | Variable directive의 Controller panel 내부의 variable 시각화와 라이프 사이클 관리 |
| `CustomInfluxDatasource` |   grafana의 datasource plugin으로 패널 내부의 variable(influxdb용) 생성에 사용    |
| `CustomInfluxQueryCtrl`  |                 CustomInflux metric editor directive의 controller                 |
| `TrendDatasource`        |          grafana의 datasource plugin으로 외부(Python) API와 연동 시 사용          |
| `TrendQueryCtrl`         |                    Trend metric editor directive의 controller                     |

#### 3.2.2.2 클래스 다이어그램

- ### Grafana-trend-panel

  ![image](https://user-images.githubusercontent.com/23473356/89762025-939ef900-db2a-11ea-89c7-d61293b01f81.png)

  ![image](https://user-images.githubusercontent.com/23473356/89762011-8c77eb00-db2a-11ea-9d3f-0e8a2a4b08a3.png)

  ![image](https://user-images.githubusercontent.com/23473356/89762218-f1334580-db2a-11ea-9bed-de69d4fe9a44.png)

- ### Grafana-trend-datasource

  ![image](https://user-images.githubusercontent.com/23473356/90706410-0b5de800-e2d0-11ea-9b68-e5ad0df439a0.png)

- ### Grafana-influx-datasource

  ![image](https://user-images.githubusercontent.com/23473356/90706548-5415a100-e2d0-11ea-9b42-146ef7daa3d1.png)

- ### Grafana-tabulator-panel

  ![image](https://user-images.githubusercontent.com/23473356/90706573-6a236180-e2d0-11ea-9ec6-a99821848c3a.png)

- ### Grafana-wafer-panel

  ![image](https://user-images.githubusercontent.com/23473356/90706626-86bf9980-e2d0-11ea-94e0-98c57f9e4faa.png)

- ### Grafana-plotly-panel

  ![image](https://user-images.githubusercontent.com/23473356/90706812-03eb0e80-e2d1-11ea-9555-129ea79d5356.png)

# 4. 시각화

## 4.1 화면 구성

### 4.1.1 메인 화면 구성

- ### Grafana

  - ### Add datasource

    - ### Datasource add page로 이동

      ![image](https://user-images.githubusercontent.com/23473356/89759423-1d4bc800-db25-11ea-877f-dbdb93e49fc9.png)

    - ### Datasource 생성

      ![image](https://user-images.githubusercontent.com/23473356/89759670-a4993b80-db25-11ea-8c00-2bd2c91d6f6f.png)

  - ### Dashboard create/export/import

    - ### Create dashboard

      ![image](https://user-images.githubusercontent.com/23473356/89758558-11f79d00-db23-11ea-89b1-057fbc48f7bc.png)

    - ### Export dashboard

      - Export modal click

        ![image](https://user-images.githubusercontent.com/23473356/89758716-67cc4500-db23-11ea-969e-16457d28f93b.png)

      - Dashborad 정보를 json 형태로 저장

        ![image](https://user-images.githubusercontent.com/23473356/89758720-6ac73580-db23-11ea-9ab8-3045497ecb7f.png)

      - Export된 dashboard.json 확인

        ![image](https://user-images.githubusercontent.com/23473356/89758725-6bf86280-db23-11ea-8e6b-d96207f0dfc0.png)

    - ### Import dashboard

      - Import page로 이동

        ![image](https://user-images.githubusercontent.com/23473356/89758797-9f3af180-db23-11ea-897b-33e465d45619.png)

      - Export 한 dashboard.json 정보 upload

        ![image](https://user-images.githubusercontent.com/23473356/89759266-bc23f480-db24-11ea-84a6-0f1efcd38c37.png)

      - Import 정보 설정

        ![image](https://user-images.githubusercontent.com/23473356/89758890-df01d900-db23-11ea-9a0c-2a6b41ce56d5.png)

      - Import 결과

        ![image](https://user-images.githubusercontent.com/23473356/89758716-67cc4500-db23-11ea-969e-16457d28f93b.png)

  - ### Add panel

    - ### Panel plugin 선택

    ![image](https://user-images.githubusercontent.com/23473356/89760048-7536fe80-db26-11ea-8c5c-f7add2d3dbfe.png)

    - ### 생성된 Panel 확인

      ![image](https://user-images.githubusercontent.com/23473356/89760356-2c337a00-db27-11ea-9b09-e8de12d9c001.png)

    - ### Edit Panel

      ![image](https://user-images.githubusercontent.com/23473356/89760217-da8aef80-db26-11ea-992f-fbadfa0d0383.png)

      - Metric

        - datasource 선택

          ![image](https://user-images.githubusercontent.com/23473356/89760568-a106b400-db27-11ea-98b4-d86ffdc9e065.png)

        - panel metric query 작성(Raw Query mode)

          ![image](https://user-images.githubusercontent.com/23473356/89760621-b5e34780-db27-11ea-942b-ed86b6247a81.png)

- ### variable

  - Variable edit

    ![image](https://user-images.githubusercontent.com/23473356/89761288-04451600-db29-11ea-85e6-baccbce13cea.png)

  - Variable 생성 화면

    ![image](https://user-images.githubusercontent.com/23473356/89761293-07400680-db29-11ea-815e-03d6fa3f0c48.png)

- Variable 사용 (Panel's metric query)

  ![image](https://user-images.githubusercontent.com/23473356/89761767-170c1a80-db2a-11ea-8b2b-3d1e80d02af0.png)

- Syntax

  `$varname`<br>

  select Gas1_Monitor from `$Module` where LOTID = `'$lotid'`

- Grafana-plugin-datasource
  - Trend-panel
  - Tabulator-panel
  - Wafermap-panel(grafana-plotly-panel)
  - plotly-panel
  - CPCPK
- Grafana-plugin-panel
  - simple-json-datasource
  - Influx-datasource
  - trend-datasource
  - measurement-datasource

## 4.2 Component 설명

### Grafana

- ### Panel

  - 데이터를 실질적으로 시각화하는 Panel

- ### Datasource

  - Panel에 사용하기 위한 Data를 API로 부터 가져오는 Rule을 설정 및 관리

- ### Variable
  - Dashboard 내부에서 api query의 결과를 gloabal 변수로 각 Panel의 query에서 사용하기 위한 기능
