# C2SMR

This is the C2SMR organisation.

Here is a little documentation of the projects

[NOTION](https://silent-turquoise-817.notion.site/public-C2SMR-96cfe5504c7f4cefa3fa0ede10975adc?pvs=25)

---

## Table of Contents
 
- [Bdd](#bdd)
- [App](#app)
- [Web](#web)
- [Detector.min](#detectormin)
- [Detector.all](#detectorall)
- [Detector](#detector)
- [Make_dataset_with_raspberrypi](#make_dataset_with_raspberrypi)
- [Add new sites](#add-new-sites)

---


## BDD

Mysql database with one schema C2SMR who contain 3 tables

- CITY
  - Name (primary key)
  - mail (null)
  - password (null)
  - latitude (float)
  - longitude (float)
  - color_flag (int) -> 0 for green ; 1 for yellow ; 2 for red
  - actual_picture (null)
  - number_beach (int)
  - number_sea (int)
- DATA
  - ID (primary key)
  - CITY (foreign key)
  - nb_beach (int)
  - nb_sea (int)
  - time (varchar(200))
  - precipitation (int)
  - temp_beach (int)
  - cloud_cover (int)
  - wind (int)
  - visibility (int)
  - cam_visibility (int) -> no weather data but the size of sea by computer vision
- WARNINGS
  - ID (primary key)
  - CITY (foreign key)
  - COLOR (int) -> 0 for green ; 1 for yellow ; 2 for red
  - information (varchar(200))
  - picture (null)
  - notif (0)

---

## App

This project is the mobile application of C2SMR develop with react native   

### Launch the project

```bash
npm start
```

### Create APK

```bash
npx eas build -p android --profile preview
```

### Deploy to the play store
Don't forget to change the version in app.json
```bash
npx eas build --platform android
```

---


## Web

This project launch the mysql db, the api and the front of c2smr.fr

### Launch the project

```bash
docker compose up --build
```

### Launch test 
```bash
docker compose -f docker-compose.test.yml up --build
```

### URL
- [api](https://api.c2smr.fr)
- [swagger](https://api.c2smr.fr/apidocs)
- [front](https://c2smr.fr)

### ENV

- RASPBERRY_KEY -> key to access to the /machine in the api

### API
- Linter : flake8
  - ```bash
    py -m flake8
    ```
- techno :
    - ![](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)
    - ![](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
- api/app.py
- route beginning by /client for get information to app
- route beginning by /machine for post information to bdd (use by detector)

### Front
- techno : 
  - ![](https://img.shields.io/badge/JavaScript-323330?style=for-the-badge&logo=javascript&logoColor=F7DF1E)
  - ![](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
- routes:
    - /* -> home
    - /privacy -> privacy policy

---

## Detector.min

This project send only the weather alert to the api for all sites in `src/city.py`

### Lint
```bash
py -m flake8
```

### Launch the project
```bash
docker compse up --build
```


### Techno
- ![](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)

---

## Detector.all

This project send all alert only for youtube scrapper to the api for all sites in `src/city.py`

### Dev


#### ADD ALERT


Go to alert.py <br>
Add your new method in run() and update detector repository
````python
    def run(self) -> None:
        self.no_one()
        self.beach_full()
        self.have_boat()
        self.rain()
        self.hard_wind()
        self.swimmer_away()
        self.hot()
        self.no_sea_detected()
````


#### Add roboflow Project


- in main.py constructor and update detector repository
```python
    self.OTHER_PROJECT_ROBOFLOW: list = [
            Roboflow(api_key="key").workspace()
            .project("project_name")
            .version(project_version).model
        ]

```


### Lint
```bash
py -m flake8
```

### Launch the project
```bash
docker compse up --build
```

### Techno
- ![](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)


---

## Detector

This project is the default detector for youtube scrapper or raspberry pi camera for only one site

### Env

- TYPE_OF_VIDEO -> vide if you want to use the raspberry pi camera or youtube url if you want to use youtube scrapper
- ROBOFLOW_VERSION -> version of the model of the first robolow project
- RASPBERRY_KEY -> the same password that the api for the /machine route

### Dev


#### ADD ALERT


Go to alert.py <br>
Add your new method in run() and update detector.all repository
````python
    def run(self) -> None:
        self.no_one()
        self.beach_full()
        self.have_boat()
        self.rain()
        self.hard_wind()
        self.swimmer_away()
        self.hot()
        self.no_sea_detected()
````


#### Add roboflow Project


- in main.py constructor and update detector.all repository
```python
    self.OTHER_PROJECT_ROBOFLOW: list = [
            Roboflow(api_key="key").workspace()
            .project("project_name")
            .version(project_version).model
        ]

```


### Lint
```bash
py -m flake8
```

### Launch the project
```bash
docker compse up --build
```

### Techno
- ![](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)


--- 

## make_dataset_with_raspberrypi

This project is for making dataset with Raspberry Pi with Opencv

## Techno

![](https://img.shields.io/badge/Python-3.10-3776AB.svg?style=for-the-badge&logo=Python)

## Launch

```bash
py main.py
```

## Lint

```bash
py -m flake8
```

## Parameters

- `time_sleep`: time to sleep between each image capture



---

## Add new sites

No functional project at this moment but you can add new sites in the bdd with datagrip for example

- in CITY
  - add new line and ignore email, password and picture column
- in DATA
  - add new line with CITY and null values
- in Warnings 
  - add new line with CITY first HI alert and Green Color (0)
