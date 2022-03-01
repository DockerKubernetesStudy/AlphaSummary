## **docker로 jupyter notebook 띄우기**

- 컨테이너 내부에서 jupyter notebook이 실행되는 폴더인 `/home/jovyan` 폴더를 호스트PC의 현재 폴더로 만듦
- host PC에서 docker를 실행하는 폴더에 있는 주피터 노트북 파일 작업이 가능하도록 함

```bash
docker run --rm -d -p 8888:8888 -v /home/ubuntu/2021_LEARN:/home/jovyan/work jupyter/datascience-notebook
```

- token을 입력하여 jupyter notebook 실행

## **컨테이너와 컨테이너 연결**

- docker run 옵션으로 `-link` 옵션을 사용하여 연결
- `-link [본래 컨테이너 이름]:[컨테이너를 가리킬 이름]`

```dockerfile
FROM mysql:5.7

# mysql 슈퍼관리자인 root ID에 대한 password 란에 원하는 패스워드 설정
ENV MYSQL_ROOT_PASSWORD=password
ENV MYSQL_DATABASE=dbname
```

- 데이터베이스 컨테이너 생성

```bash
# docker 이미지 작성하기
docker build --tag mysqldb -f Dockerfile_MYSQL .

# 컨테이너 생성하기 3306 포트에 연결하고, volume 옵션 설정
docker run -d -p 3306:3306 --name mydb -v /home/ubuntu/mysqldata:/var/lib/mysql mysqldb
```

- `-link` 옵션을 사용해서 주피터 노트북 컨테이너 실행하기

```bash
# --link 컨테이너이름:연결할컨테이너를지칭할이름
docker run --rm -d -p 8888:8888 -v /home/ubuntu/2021_LEARN:/home/jovyan/work --link mydb:myupyterdb jupyter/datascience-notebook
```